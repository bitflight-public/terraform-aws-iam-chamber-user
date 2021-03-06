# terraform-aws-iam-chamber-user [![Build Status](https://travis-ci.org/cloudposse/terraform-aws-iam-chamber-user.svg?branch=master)](https://travis-ci.org/cloudposse/terraform-aws-iam-chamber-user)

Terraform module to provision a basic IAM [chamber](https://github.com/segmentio/chamber) user with access to SSM parameters and KMS key to decrypt secrets, suitable for CI/CD systems
(_e.g._ TravisCI, CircleCI, CodeFresh) or systems which are *external* to AWS that cannot leverage [AWS IAM Instance Profiles](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_use_switch-role-ec2_instance-profiles.html).

We do not recommend creating IAM users this way for any other purpose.


## Usage

```terraform
module "chamber_user" {
  source        = "git::https://github.com/cloudposse/terraform-aws-iam-chamber-user.git?ref=master"
  namespace     = "cp"
  stage         = "prod"
  name          = "chamber"
  kms_key_alias = "alias/parameter_store_key"
}

module "kms_key" {
  source                  = "git::https://github.com/cloudposse/terraform-aws-kms-key.git?ref=master"
  namespace               = "cp"
  stage                   = "prod"
  name                    = "key"
  description             = "KMS key for chamber"
  deletion_window_in_days = 10
  enable_key_rotation     = "true"
  alias                   = "alias/parameter_store_key"
}
```


## Variables

| Name            | Default | Description                                                                                 | Required |
|:----------------|:-------:|:--------------------------------------------------------------------------------------------|:--------:|
| `namespace`     |   ``    | Namespace (e.g. `cp` or `cloudposse`)                                                       |   Yes    |
| `stage`         |   ``    | Stage (e.g. `prod`, `dev`, `staging`)                                                       |   Yes    |
| `name`          |   ``    | Name  (e.g. `app`)                                                                          |   Yes    |
| `kms_key_alias` |   `alias/parameter_store_key`    | KMS key alias used to decrypt secrets in Parameter Store                                    |   Yes    |
| `attributes`    |  `[]`   | Additional attributes (e.g. `1`)                                                            |    No    |
| `tags`          |  `{}`   | Additional tags  (e.g. `map("BusinessUnit","XYZ")`                                          |    No    |
| `delimiter`     |   `-`   | Delimiter to be used between `namespace`, `stage`, `name` and `attributes`                  |    No    |
| `force_destroy` | `false` | Destroy even if it has non-Terraform-managed IAM access keys, login profiles or MFA devices |    No    |
| `path`          |   `/`   | Path in which to create the user                                                            |    No    |
| `enabled`       | `true`  | Set to `false` to prevent the module from creating any resources                            |    No    |
| `ssm_actions`   | `["ssm:GetParametersByPath", "ssm:GetParameters"]`   | Actions to allow in the policy                   |    No    |
| `ssm_resources` | `["*"]` | Resources to apply the actions specified in the policy                                      |    No    |


## Outputs

| Name                | Description                                                                 |
|:--------------------|:----------------------------------------------------------------------------|
| `user_name`         | Normalized IAM user name                                                    |
| `user_arn`          | The ARN assigned by AWS for the user                                        |
| `user_unique_id`    | The user unique ID assigned by AWS                                          |
| `access_key_id`     | The access key ID                                                           |
| `secret_access_key` | The secret access key. This will be written to the state file in plain-text |


## Help

**Got a question?**

File a GitHub [issue](https://github.com/cloudposse/terraform-aws-iam-chamber-user/issues), send us an [email](mailto:hello@cloudposse.com) or reach out to us on [Gitter](https://gitter.im/cloudposse/).

## Contributing

### Bug Reports & Feature Requests

Please use the [issue tracker](https://github.com/cloudposse/terraform-aws-iam-chamber-user/issues) to report any bugs or file feature requests.

### Developing

If you are interested in being a contributor and want to get involved in developing `terraform-aws-iam-chamber-user`, we would love to hear from you! Shoot us an [email](mailto:hello@cloudposse.com).

In general, PRs are welcome. We follow the typical "fork-and-pull" Git workflow.

 1. **Fork** the repo on GitHub
 2. **Clone** the project to your own machine
 3. **Commit** changes to your own branch
 4. **Push** your work back up to your fork
 5. Submit a **Pull request** so that we can review your changes

**NOTE:** Be sure to merge the latest from "upstream" before making a pull request!


## License

[APACHE 2.0](LICENSE) © 2018 [Cloud Posse, LLC](https://cloudposse.com)

See [LICENSE](LICENSE) for full details.

    Licensed to the Apache Software Foundation (ASF) under one
    or more contributor license agreements.  See the NOTICE file
    distributed with this work for additional information
    regarding copyright ownership.  The ASF licenses this file
    to you under the Apache License, Version 2.0 (the
    "License"); you may not use this file except in compliance
    with the License.  You may obtain a copy of the License at

      http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing,
    software distributed under the License is distributed on an
    "AS IS" BASIS, WITHOUT WARRANTIES OR CONDITIONS OF ANY
    KIND, either express or implied.  See the License for the
    specific language governing permissions and limitations
    under the License.


## About

`terraform-aws-iam-chamber-user` is maintained and funded by [Cloud Posse, LLC][website].

![Cloud Posse](https://cloudposse.com/logo-300x69.png)

Like it? Please let us know at <hello@cloudposse.com>

We love [Open Source Software](https://github.com/cloudposse/)!

See [our other projects][community]
or [hire us][hire] to help build your next cloud platform.

  [website]: https://cloudposse.com/
  [community]: https://github.com/cloudposse/
  [hire]: https://cloudposse.com/contact/


## Contributors

| [![Erik Osterman][erik_img]][erik_web]<br/>[Erik Osterman][erik_web] | [![Andriy Knysh][andriy_img]][andriy_web]<br/>[Andriy Knysh][andriy_web] |  [![Sarkis Varozian][sarkis_img]][sarkis_web]<br/>[Sarkis Varozian][sarkis_web] |
|------------------------------|------------------------------|------------------------------|

  [erik_img]: http://s.gravatar.com/avatar/88c480d4f73b813904e00a5695a454cb?s=144
  [erik_web]: https://github.com/osterman/
  [andriy_img]: https://avatars0.githubusercontent.com/u/7356997?v=4&u=ed9ce1c9151d552d985bdf5546772e14ef7ab617&s=144
  [andriy_web]: https://github.com/aknysh/
  [sarkis_img]: https://avatars3.githubusercontent.com/u/42673?s=144&v=4
  [sarkis_web]: https://github.com/sarkis/
