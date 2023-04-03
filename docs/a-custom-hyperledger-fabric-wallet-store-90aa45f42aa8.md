# 自定义 Hyperledger 结构钱包商店

> 原文：<https://medium.com/coinmonks/a-custom-hyperledger-fabric-wallet-store-90aa45f42aa8?source=collection_archive---------2----------------------->

![](img/4ae0b1b98e6ed90835f664344983a55b.png)

# 先决条件

本教程要求读者对 Hyperledger Fabric 有一个基本的了解。如果你是织物的初学者，我真的建议你好好看看它的[文档](https://hyperledger-fabric.readthedocs.io/en/release-2.2/)。按照[【入门】](https://hyperledger-fabric.readthedocs.io/en/release-2.2/getting_started.html)中描述的步骤，您应该能够与两到三个组织一起运行 fabric 测试网络。

**调出测试网络**

您可以在`fabric-samples`存储库的`test-network`目录中找到启动网络的脚本。使用以下命令导航到测试网络目录:

```
cd fabric**-**samples**/**test**-**network
```

在这个目录中，您可以找到一个带注释的脚本，`network.sh`，它使用本地机器上的 Docker 映像建立了一个 Fabric 网络。您可以运行`./network.sh -h`来打印脚本帮助文本，或者使用以下命令启动测试网络

```
./network.sh up createChannel -ca -c mychannel -s couchdb
```

这将创建一个网络，其中有一个名为“mychannel”的通道，带有 CA(证书颁发机构)和 CouchDB。

# 背景

那么这个教程的目的是什么呢？

> 本教程帮助读者为 Fabric 创建一个自定义钱包商店。

但是为什么我们需要一个定制钱包商店呢？安装 Hyperledger Fabric 并浏览其示例源代码后，您可能会发现非常有趣的一点是，您的钱包信息存储在**文件**中。让我们看看`fabric-samples`文件夹中一个众所周知的`fabcar`的例子。在我们安装了应用程序依赖项之后，我们可以使用 Node.js SDK
来创建我们的监听器应用程序将用来与
网络交互的身份。运行以下命令注册管理员用户:

```
node enrollAdmin.js
```

将在`wallet`文件夹下创建一个名为`admin.id`的文件来存储管理员的身份。该身份将用于代表测试网络管理员与 Hyperledger 进行交互。

**一个问题**是，在 web 服务器文件系统上存储身份不是一个好主意。如果我们需要将单个 web 服务器扩展到多个实例，并将它们放在 web 负载平衡器后面，该怎么办？身份文件需要在所有服务器之间同步，以使请求成为可能，这非常麻烦。

```
We need to store user's identity in a central database (although by doing this we lost the decentralization). 
```

除了我们目前使用 FileSystemWalletStore 之外，Fabric 还支持另外两种钱包:

*   CouchDB 钱包商店
*   内存中的钱包存储

※参考资料:[https://github . com/hyperledger/fabric-SDK-node/tree/7 a1 eed FB 3d 13 a 1797 FD 9 FB 4 a7 A8 ca 34 c 46733d 06/fabric-network/src/impl/wallet](https://github.com/hyperledger/fabric-sdk-node/tree/7a1eedfb3d13a1797fd9fb4a7a8ca34c46733d06/fabric-network/src/impl/wallet)

这些钱包存储作为中间的存储库，帮助应用程序将用户的钱包存储在 CouchDB 或内存中。

```
The question is : What if we need MySQL wallet store or Postgres wallet store? 
```

# 实现自定义钱包存储

在我的例子中，我和我的团队在 NetsJS api 服务器上工作，它有 Postgres 数据库。我们希望将用户的钱包存储在 Postgres 中，而不是文件系统或沙发数据库或内存中。这一需求导致我们需要实现一个支持 Postgres DB 的自定义钱包商店。起初这似乎很难，但随着深入研究，我们发现实现这种事情是很容易的。首先，我们来看看所有 Wallet Store 实现都必须遵循的钱包商店接口:

```
/// <reference types="node" />
export interface WalletStore {
    get(label: string): Promise<Buffer | undefined>;
    list(): Promise<string[]>;
    put(label: string, data: Buffer): Promise<void>;
    remove(label: string): Promise<void>;
}
```

很简单，不是吗？只需要实现 4 个方法

*   **get** ():获取与给定标签相关的数据。
*   **列表**():列出所有存储数据的标签
*   **put** ():放置与给定标签相关的数据。
*   **删除**():删除与给定标签相关的数据。

因此，如果执行这 4 个函数，我们需要访问存储钱包的 Postgres 表。在我们的例子中，一个名为 table 的用户将存储所有用户信息，它的`wallet`字段用于存储相应的他/她的身份。

```
import { Entity, Column, PrimaryGeneratedColumn } from 'typeorm';

@Entity({ name: 'users' })
export class User {
  @PrimaryGeneratedColumn()
  public id: number;

  @Column({ unique: true })
  public email: string;

  @Column({ select: false })
  public password?: string;

  @Column()
  public name: string;

  @Column({ select: false, nullable: true })
 **public wallet: string;** 
  @Column({ name: 'display_name', nullable: true })
  public displayName: string;

  @Column({ type: 'text', nullable: true })
  public avatar: string;

  @Column({ nullable: true, type: 'date' })
  public dob: Date;

  @Column({ nullable: true })
  public address: string;
}
```

通过使用 userRepository 作为与表交互的方法，wallet store 的实现将会很简单。请注意:

*   我们使用电子邮件(唯一的)作为寻找钱包的标签
*   由于默认情况下`wallet`是`select:false`,因此 select 查询不会检索到它，这就是为什么我们需要手动显式添加`wallet`。

```
import { User } from '../user/entities/user.entity';
import { Repository } from 'typeorm';
import { WalletStore } from 'fabric-network';

export class DbWalletStore implements WalletStore {
  constructor(private readonly userRepository: Repository<User>) {}

  public async remove(email: string): Promise<any> {
    const user: User = await this.userRepository.findOneOrFail({
      where: { email: email },
    });
    user.wallet = null;
    try {
      await this.userRepository.save(user);
      return true;
    } catch (e) {
      return false;
    }
  }

  public async get(email: string): Promise<Buffer> {
    const user: User = await this.userRepository
      .createQueryBuilder('users')
      .select('users')
      .where({ email: email })
      .addSelect('users.wallet')
      .getOneOrFail();
    return user.wallet ? ***Buffer***.from(user.wallet, 'utf-8') : null;
  }

  public async list(): Promise<string[]> {
    const users: User[] = await this.userRepository
      .createQueryBuilder('users')
      .select('users')
      .addSelect('users.wallet')
      .getMany();
    return users.map((user) => {
      return user.wallet;
    });
  }

  public async put(email: string, data: Buffer): Promise<any> {
    const user: User = await this.userRepository.findOneOrFail({
      where: { email: email },
    });
    user.wallet = data.toString('utf8');
    try {
      await this.userRepository.save(user);
      return true;
    } catch (e) {
      return false;
    }
  }
}
```

要使用 DBWalletStore，我们需要在它的构造函数中注入`userRepository`。为此，我们创建了一个名为 fabric 的模块，并在 fabric 服务中启动钱包存储。

***fabric . module . ts***

```
import { Module } from '@nestjs/common';
import { FabricService } from './fabric.service';
import { DbWalletStore } from './dbwallet-store';
import { TypeOrmModule } from '@nestjs/typeorm';
import { User } from '../user/entities/user.entity';
import { ConfigModule, ConfigService} from '@nestjs/config';

@Module({
  imports: [TypeOrmModule.*forFeature*([User]), ConfigModule],
  providers: [DbWalletStore, FabricService, ConfigService],
  exports: [FabricService, ConfigService],
})
export class FabricModule {}
```

**T20**

```
export class FabricService {
  */**
   * Inject User repository for R/W wallet information
   */* private readonly connectionConfigurations: Record<string, any>[];
  private readonly walletStore: DbWalletStore;

  constructor(
    @InjectRepository(User)
    private readonly userRepository: Repository<User>,
    private configService: ConfigService,
  ) {
 **this.walletStore = new DbWalletStore(this.userRepository);**  }
```

现在，您可以将示例中使用 FileSystemWalletStore 的代码替换为我们的自定义钱包存储:

替换这个:

```
const walletPath = path.join(process.cwd(), 'wallet');
const wallet = await Wallets.newFileSystemWallet(walletPath); 
```

通过这个:

```
const wallet = new Wallet(this.walletStore);
```

我们不再使用 Wallet 类，因为如果您查看源代码，您会发现 Wallet 实际上遵循工厂模式，除了创建一个 Wallet 实例之外什么也不做

```
/**
  * Create a wallet backed by the provided file system directory.
  * [@param](http://twitter.com/param) {string} directory A directory path.
  * [@returns](http://twitter.com/returns) {Promise<module:fabric-network.Wallet>} A wallet.
  */
 public static async newFileSystemWallet(directory: string): Promise<Wallet> {
  const store = await FileSystemWalletStore.newInstance(directory);
  return new Wallet(store);
 }
```

# #结束

感谢 KC Tam 先生关于 Hyperledger Fabric 的有价值的系列文章，它激励我写这篇教程。

在下一篇教程中，我将向您展示如何使用 NestJS 以更简洁的方式与 Hyperledger Fabric 进行交互。尽情享受吧！

> 交易新手？试试[密码交易机器人](/coinmonks/crypto-trading-bot-c2ffce8acb2a)或者[复制交易](/coinmonks/top-10-crypto-copy-trading-platforms-for-beginners-d0c37c7d698c)