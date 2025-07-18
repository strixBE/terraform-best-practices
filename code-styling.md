# Code styling

{% hint style="info" %}
* Examples and Terraform modules should contain documentation explaining features and how to use them.
* All links in README.md files should be absolute to make Terraform Registry website show them correctly.
* Documentation may include diagrams created with [mermaid](https://github.com/mermaid-js/mermaid) and blueprints created with [cloudcraft.co](https://cloudcraft.co).
* Use [Terraform pre-commit hooks](https://github.com/antonbabenko/pre-commit-terraform) to make sure that the code is valid, properly formatted, and automatically documented before it is pushed to git and reviewed by humans.
{% endhint %}

## Formatting

Terraform’s `terraform fmt` command enforces the canonical style for configuration files. The tool is intentionally opinionated and non-configurable, guaranteeing a uniform format across codebases so reviewers can focus on substance rather than style. Integrate it with [Terraform pre-commit hooks](https://github.com/antonbabenko/pre-commit-terraform) to validate and format code automatically before it reaches version control.

For example:

```yaml
# .pre-commit-config.yaml
repos:
  - repo: https://github.com/antonbabenko/pre-commit-terraform
    rev: v1.99.4
    hooks:
      - id: terraform_fmt
```

In CI pipelines, use `terraform fmt -check` to verify compliance. It exits with status 0 when all files are correctly formatted; otherwise, it returns a non-zero code and lists the offending files. Centralizing formatting in this way removes merge friction and enforces a consistent standard across teams.

## Editor Configuration

- **Use `.editorconfig`**: [EditorConfig](https://editorconfig.org/) helps maintain consistent coding styles for multiple developers working on the same project across various editors and IDEs. Include an `.editorconfig` file in your repositories to maintain consistent whitespace and indentation.

**Example `.editorconfig`:**

```editorconfig
[*]
indent_style = space
indent_size = 2
trim_trailing_whitespace = true

[*.{tf,tfvars}]
indent_style = space
indent_size = 2

[Makefile]
indent_style = tab
```

## Documentation

### Automatically generated documentation

[pre-commit](https://pre-commit.com/) is a framework for managing and maintaining multi-language pre-commit hooks. It is written in Python and is a powerful tool to do something automatically on a developer's machine before code is committed to a git repository. Normally, it is used to run linters and format code (see [supported hooks](https://pre-commit.com/hooks.html)).

With Terraform configurations `pre-commit` can be used to format and validate code, as well as to update documentation.

Check out the [pre-commit-terraform repository](https://github.com/antonbabenko/pre-commit-terraform/blob/master/README.md) to familiarize yourself with it, and existing repositories (eg, [terraform-aws-vpc](https://github.com/terraform-aws-modules/terraform-aws-vpc)) where this is used already.

### terraform-docs

[terraform-docs](https://github.com/segmentio/terraform-docs) is a tool that does the generation of documentation from Terraform modules in various output formats. You can run it manually (without pre-commit hooks), or use [pre-commit-terraform hooks](https://github.com/antonbabenko/pre-commit-terraform) to get the documentation updated automatically.


### Comment style

Use `#` for comments. Avoid `//` or block comments.

**Example:**

```hcl
# This is a comment explaining the resource
resource "aws_instance" "this" {
# ...
}
```

**Section Headers**: Delimit section headers in code with `# -----` or `######` for clarity.

**Example:**

```hcl
# --------------------------------------------------
# AWS EC2 Instance Configuration
# --------------------------------------------------

resource "aws_instance" "this" {
# ...
}
```

@todo: Document module versions, release, GH actions

## Resources

1. [pre-commit framework homepage](https://pre-commit.com/)
2. [Collection of git hooks for Terraform to be used with pre-commit framework](https://github.com/antonbabenko/pre-commit-terraform)
3. Blog post by [Dean Wilson](https://github.com/deanwilson): [pre-commit hooks and terraform - a safety net for your repositories](https://www.unixdaemon.net/tools/terraform-precommit-hooks/)
