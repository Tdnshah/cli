# Contributing

## General guidelines

- Issues filed directly with this project aren’t subject to a service-level agreement (SLA).
- The project maintainers are under no obligation to respond to support requests, feature requests, or pull requests.
- If more information is requested and no reply is received within a week, issues may be closed.

Newly filed issues will be reviewed by a maintainer and added to the backlog milestone if accepted.

Acquia doesn’t publish timelines or road maps to reflect when individual issues will be addressed. If you would like to request prioritization of a specific ticket, complete the following tasks:

- Submit product feedback through your Technical Account Manager or [submit a support ticket](https://docs.acquia.com/support/#contact-acquia-support) on your Cloud subscription.
- Vote for the relevant issue by adding a +1 reaction.
- Submit a pull request, which will receive priority review.

## Submitting issues

Before submitting an issue, be sure to search for existing issues (including closed issues) matching your issue. Duplicate issues will be closed.

Take care when selecting your issue type, and if you aren’t sure of the issue type, consider submitting a support request.

- Feature request: A request for a specific enhancement for this project. A feature request is distinct from a bug report because it indicates a missing feature for this project instead of a literal error with this project. Feature requests are distinct from support requests because they’re specific and atomic requests for new this project features, instead of a general request for help or guidance.

- Bug report: A defined instance of this project not behaving as expected. A bug report is distinct from a feature request because it represents a mismatch between what this project does and what this project claims to do. A bug report is distinct from a support request by including specific steps to reproduce the problem (ideally starting from a fresh installation of this project) and justifying why the instance is a problem with this project rather than with an underlying tool, such as Composer or Drush.

- Support request: A request for help or guidance. Use the issue type if you aren’t sure how to do something or can’t find a solution to a problem that may or may not be a bug. Before filing a support request, review documentation for solutions to common problems and general troubleshooting techniques.

If you have an Acquia subscription, consider filing a support ticket instead of an issue to receive support subject to your SLA.

After selecting your issue type, be sure to complete the entire issue template.

## Submitting pull requests

Pull requests must also adhere to the following guidelines:

- Pull requests must be atomic and targeted at a single issue rather than broad scope.
- Pull requests must contain clear testing steps and justification, and all other information required by the pull request template.
- Pull requests must pass automated tests before they will be reviewed. Acquia recommends running the tests locally before submitting.
- Pull requests must meet Drupal coding standards and best practices as defined by the project maintainers.

### Automatic dev builds

Every commit on the Acquia CLI repository, including for pull requests, automatically builds and uploads acli.phar to transfer.sh in order to assist with reviews. To download acli.phar for any commit:

1. Click the "build-release" workflow link.
2. Expand the "Upload to transfer.sh" section.
3. Copy the transfer.sh and use it to download acli.phar for that commit.

## Building and testing

No special tools or dependencies are necessary to develop or contrib to Acquia CLI. Simply clone the Git repo and install Composer dependencies:
```
git clone git@github.com:acquia/cli.git
cd cli
composer install
./bin/acli
```

Be sure to validate and test your code locally using the provided Composer test scripts (`composer test`) before opening a PR.

### Building acli.phar

To test changes in production mode, build and run `acli.phar` using this process. The _build-release_ stage of [`.github/workflows/ci.yml`](.github/workflows/ci.yml) follows a similar process.

1. Install Composer production dependencies: `composer install --no-dev --optimize-autoloader`
1. Clear and rebuild your Symfony caches: `./bin/acli ckc && ./bin/acli cc`
1. Install Box (only need to do this once): `composer box-install`
1. Compile phar: `composer box-compile`

### Testing the `update` command

Any changes to the `acli update` command should be manually tested using the following steps:

1. Replace `@package_version@` on this line with `1.0.0` or any older version string: https://github.com/acquia/cli/blob/v1.0.0/bin/acli#L84
1. Build acli.phar as described above.
1. Now test: `./build/acli.phar self:update`

## Updating Cloud Platform API spec

Acquia CLI stores a local copy of the Cloud Platform API spec in the `assets` directory. To update the Cloud Platform API spec, run:

```
composer update-cloud-api-spec
```

## Generate docs

To generate docs for all commands in RST format, run:
```
./bin/acli self:make-docs
```

To copy the output easily to the clipboard, run:
```
./bin/acli self:make-docs | pbcopy
```

If you're on Mac, you can render and view the outputted RST using a command like this:
```
brew install restview
./bin/acli self:make-docs > /tmp/acli.rst && restview /tmp/acli.rst
```

## Style guide

Code, comment, and other style standards should generally follow those set by upstream projects, especially [Drupal](https://www.drupal.org/docs/develop/standards), [Symfony](https://symfony.com/doc/current/contributing/code/standards.html), and [ORCA](https://github.com/acquia/coding-standards-php). PHPCodeSniffer enforces many of these standards.

Organize commands by topic (noun) first and action (verb) second, separated by a colon (`ide:create`). Write command descriptions in sentence case and imperative mood without a trailing period (`Create a Cloud IDE`).
