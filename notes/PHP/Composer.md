## Composer



#### 介绍

Composer 是 PHP 的依赖管理工具，允许开发者轻松管理项目的库和依赖关系。通过 Composer，可以定义项目所需的依赖包，自动安装、更新这些包，并处理它们之间的版本冲突。它使用 `composer.json` 文件来描述项目的依赖项，并生成 `composer.lock` 文件以锁定确切的依赖版本，确保团队中每个人使用相同的库版本。



#### 官网

https://getcomposer.org/



#### Packagist

Packagist 是 PHP 的一个包管理器，主要用于托管和分发 PHP 库和组件。开发者可以在 Packagist 上找到各种开源 PHP 包，并通过 Composer 轻松地将它们添加到自己的项目中。网站上提供了包的版本信息、依赖关系和使用说明等。

此外，开发者也可以将自己的 PHP 包发布到 Packagist，方便其他人使用。总体来说，Packagist 是 PHP 生态系统中一个重要的资源库。

https://packagist.com/



#### 命令介绍

- composer install

  `composer install` 用于根据 `composer.lock` 文件安装项目所需的依赖包，确保使用锁定的版本。如果你只是想安装当前项目的依赖而不更改版本，使用 `composer install` 是更安全的选择；如果需要更新某些包或全部包，则使用 `composer update`。

- composer update

  `composer update` 会更新依赖包到 `composer.json` 中指定的最新版本，并重新生成 `composer.lock` 文件