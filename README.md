# foal-demo
foal小马驹，用于创建Web应用程序的Node.js框架

# 一、创建新项目

```base
npm install -g @foal/cli

foal createapp my-app
```
### 目录说明

```base
my-app/
  config/         目录包含不同环境（生产、测试​​、开发、e2e 等）的配置文件。
  node_modules/   目录包含项目的所有 prod 和 dev 依赖项。
  public/         通常是图像、CSS 和客户端 JavaScript 文件，并在服务器运行时直接提供。
  src/            目录包含应用程序的所有源代码。
    app/          目录包括服务器的组件（控制器、服务和钩子）。
    e2e/          端到端测试。
    scripts/      包含旨在从命令行调用的脚本（例如：create-user）。
  ormconfig.js    定义了数据库连接的配置和凭据。它们也可以通过环境变量传递。
  package.json    列出的依赖关系和项目的命令。
  tsconfig.*.json 列出了每个npm命令的 TypeScript 编译器配置。
  .eslintrc.js    linting 配置。
```

# 二、启动服务器

```base
cd my-app

npm run develop
```

# 三、创建 Model

> FoalTS 使用 TypeORM, 其中 `Entity` 是 `TypeORM` 的装饰器修饰的Class

```base
foal generate entity todo
```

> 打开目录 `todo.entity.ts` 中的 `src/app/entities` 文件并添加 `text` 列

```base
import { BaseEntity, Column, Entity, PrimaryGeneratedColumn } from 'typeorm';

@Entity()
export class Todo extends BaseEntity {

  @PrimaryGeneratedColumn()
  id: number;

  @Column()
  text: string;

}
```

# 四、创建数据库表

> 使用数据库软件，或使用`迁移（一种更新数据库架构的编程方式）`。使用迁移的优点是您可以直接从实体定义中创建、更新和删除表。

> 生成迁移文件

```base
npm run makemigrations
```

> `src/migrations/` 目录中出现一个新文件, 该文件包含在迁移期间执行所有的 `SQL` 查询。

```base
import {MigrationInterface, QueryRunner} from "typeorm";

export class migration1562755564200 implements MigrationInterface {

    public async up(queryRunner: QueryRunner): Promise<void> {
        await queryRunner.query(`CREATE TABLE "todo" ("id" integer PRIMARY KEY AUTOINCREMENT NOT NULL, "text" varchar NOT NULL)`, undefined);
        await queryRunner.query(`CREATE TABLE "user" ("id" integer PRIMARY KEY AUTOINCREMENT NOT NULL)`, undefined);
    }

    public async down(queryRunner: QueryRunner): Promise<void> {
        await queryRunner.query(`DROP TABLE "user"`, undefined);
        await queryRunner.query(`DROP TABLE "todo"`, undefined);
    }

}
```

> 运行迁移，TypeORM 检查之前运行过的所有迁移（在本例中没有）并执行新的迁移。

```base
npm run migrations
```

> 数据库 ( `db.sqlite3`) 现在包含一个名为 `todo` 的新表：

```base
+------------+-----------+-------------------------------------+
|                             todo                             |
+------------+-----------+-------------------------------------+
| id         | integer   | PRIMARY KEY AUTO_INCREMENT NOT NULL |
| text       | varchar   | NOT NULL                            |
+------------+-----------+-------------------------------------+
```


