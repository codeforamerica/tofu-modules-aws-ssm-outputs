# AWS SSM Outputs Module

[![Main Checks][badge-checks]][code-checks] [![GitHub Release][badge-release]][latest-release]

This module provides an interface for writing outputs from your OpenTofu
configuration to AWS Systems Manager (SSM) Parameter Store, so that they can be
used as inputs for other configurations. Along with the [aws_ssm_inputs] module,
it allows you to easily manage and access configuration parameters stored in
SSM.

## Usage

> [!WARNING]
> The values stored as outputs by this module are assumed to be insecure, and
> are stored as plain text. You should not use this module to store sensitive
> values.

Add this module to your `main.tf` (or appropriate) file and configure the inputs
to match your desired configuration. For example:

```hcl
module "module_name" {
  source = "github.com/codeforamerica/tofu-modules-aws-ssm-outputs?ref=1.0.0"

  prefix = "/my-project/environment"
  outputs = {
    "logging/key"         = module.logging.kms_key_arn
    "vpc/id"              = module.vpc.vpc_id
    "vpc/private-subnets" = join(",", module.vpc.private_subnets)
  }
}
```

Make sure you re-run `tofu init` after adding the module to your configuration.

```bash
tofu init
tofu plan
```

To update the source for this module, pass `-upgrade` to `tofu init`:

```bash
tofu init -upgrade
```

## Inputs

| Name    | Description                                                         | Type     | Default | Required |
|---------|---------------------------------------------------------------------|----------|---------|----------|
| outputs | Map of parameter names to their values to be stored.                | `map`    | n/a     | yes      |
| prefix  | Prefix for all parameters. Should start with a forward slash (`/`). | `string` | `null`  | no       |
| tags    | Optional tags to be applied to all resources.                       | `map`    | `{}`    | no       |


## Contributing

Follow the [contributing guidelines][contributing] to contribute to this
repository.

[aws_ssm_inputs]: https://github.com/codeforamerica/tofu-modules-aws-ssm-inputs
[badge-checks]: https://github.com/codeforamerica/tofu-modules-aws-ssm-outputs/actions/workflows/main.yaml/badge.svg
[badge-release]: https://img.shields.io/github/v/release/codeforamerica/tofu-modules-aws-ssm-outputs?logo=github&label=Latest%20Release
[code-checks]: https://github.com/codeforamerica/tofu-modules-aws-ssm-outputs/actions/workflows/main.yaml
[contributing]: CONTRIBUTING.md
[latest-release]: https://github.com/codeforamerica/tofu-modules-aws-ssm-outputs/releases/latest
