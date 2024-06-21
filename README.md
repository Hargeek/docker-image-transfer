# Docker Image Transfer

本项目参考了 [docker_image_pusher](https://github.com/tech-shrimp/docker_image_pusher) 项目，感谢原作者的贡献。

在原项目的基础上，本项目主要增加/重构了以下核心功能以提高使用体验：

- [x] 快速得到转存镜像地址：
    - 将内置acr相关参数中的`ALIYUN_REGISTRY`、`ALIYUN_NAME_SPACE`修改为`Variable`方式
    - 改造了原项目维护`images.txt`文件处理镜像地址的存储，若其他用户不`fork`本项目并配置相应的`secrets`，就需要通过`pr`
      的方式提交镜像地址，`pr`还需要原作者合并后才能触发转存
    - 本项目改为通过`issue comment`触发`docker`镜像的传输及转存，用户只需要在`issue`中提交镜像地址即可触发转存并得到转存后的镜像地址
    - 转存后的镜像地址会自动通过`issue comment`回复给用户

## 说明

把说明放到前面，方便用户快速了解使用本项目的注意点，避免用户在使用过程中出现问题、滥用等情况（纯属公益，不要滥用）

- `issue`中提交的镜像地址必须是公开的，否则无法下载
- 本项目仅用于传输镜像，不提供存储服务，转存后的镜像地址有效期为`30`天
- 由于`acr`个人版本的限制：全球地域下仓库默认限额为`300`，因此`30`天内的转存镜像数量有限
- 转存后的镜像地址会自动通过`issue comment`回复给用户
- 所有转存后的镜像地址会自动更新到[images.txt](images.txt)文件中，方便用户统一查看，如果不需要转存镜像，可以从此文件中查看现存的所有镜像地址列表
- 本项目不提供任何保证，如有问题请提交您的`issue`

## 镜像命名规范

### 原镜像名称说明

- `docker hub`官方，`docker hub`上的镜像命名一般有两种格式：
    - 一种是[Official Images](https://hub.docker.com/search?image_filter=official)
      ，这类镜像的命名格式为`<image_name>:<image_tag>`，例如`nginx:1.27.0-alpine`
    - 一种是非`Official`的镜像，这类镜像的命名格式为`<image_ns>/<image_name>:<image_tag>`
      ，例如`rancher/rancher-agent:v2.9-head`
    - 官方镜像有个缺省域名，`docker.io`是`docker hub`
      的默认域名，在使用时可以添加也可以忽略，例如`docker.io/nginx:1.27.0-alpine`和`nginx:1.27.0-alpine`是等效的
- 第三方其他镜像
    - 第三方的镜像命名格式一般为`<domain>/<image_ns>/<image_name>:<image_tag>`
      ，例如`registry.k8s.io/sig-storage/livenessprobe:v2.11.0`
      也可能出现多级`namespace`，例如`<domain>/<image_ns1>/<image_ns2>/<image_name>:<image_tag>`

### 转存镜像命名规范

由于`acr`只支持一级镜像名称的`namespace`，因此转存镜像的命名规范为：

- 镜像来源是`docker hub`，则转存后的镜像名称为`image:tag`，自动忽略原镜像地址中的`docker.io/`前缀
    - `nginx:latest` 转存后的镜像名称为 `nginx:latest`
    - `docker.io/nginx:latest` 转存后的镜像地址为 `nginx:latest`
- 镜像来源为第三方其他仓库，且可能出现多级`namespace`，则转存后的镜像地址为`namespace_image:tag`，自动将原镜像地址中的`/`
  替换为`_`，
    - `xxx.com/namespace/image:tag` 转存后的镜像地址为 `namespace_image:tag`
    - `xxx.com/namespace1/namespace2/image:tag` 转存后的镜像地址为 `namespace1_namespace2_image:tag`

### 表格参照

如果对上述说明不太理解，可以参考以下更为直观的示例

| 原镜像地址                                               | 转存后的镜像地址（需补充acr的域名和namespace）       |
|-----------------------------------------------------|-------------------------------------|
| `nginx:latest`                                      | `nginx:latest`                      |
| `docker.io/nginx:latest`                            | `nginx:latest`                      |
| `registry.k8s.io/sig-storage/livenessprobe:v2.11.0` | `sig-storage_livenessprobe:v2.11.0` |
| `xxx.com/namespace/image:tag`                       | `namespace_image:tag`               |
| `xxx.com/namespace1/namespace2/image:tag`           | `namespace1_namespace2_image:tag`   |

### Fork 本项目

本项目出发点是为了方便其他小白用户快速得到转存后的镜像地址，仅仅需要在`issue`中提交镜像地址即可

当然，如果想自用，也可以`fork`本项目并将项目设置为`private`，然后配置相应的`secrets`和`variable`，这样可以作为私有的转存镜像工具使用，更为灵活方便的实现镜像转存

### 关注和联系

如果觉得本项目对您有帮助，可以`star`本项目

如果有问题或建议，可以通过我的[E-mail](mailto:ssgeek@hotmail.com)联系我

我的博客地址：[山山仙人博客](https://www.ssgeek.com)
我的微信公众号：「仙人技术」，欢迎关注（底部菜单可以添加我的微信）

## TODO

- [ ] ...