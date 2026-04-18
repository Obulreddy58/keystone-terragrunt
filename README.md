# keystone-terragrunt

Terragrunt Jinja2 templates, scaffolding, and blueprint definitions for the Keystone IDP.

## Structure

```
.
├── blueprints.json            # Blueprint definitions: service-type → modules
├── scaffolding/               # Hierarchy/bootstrap templates
│   ├── root-terragrunt.hcl.j2
│   ├── account.hcl.j2
│   ├── env.hcl.j2
│   └── region.hcl.j2
├── vpc/                       # Per-service terragrunt templates
│   └── vpc/
│       └── terragrunt.hcl.j2
├── eks-cluster/
│   ├── vpc/terragrunt.hcl.j2
│   ├── eks/terragrunt.hcl.j2
│   └── eks-addons/terragrunt.hcl.j2
├── rds-database/
│   └── rds/terragrunt.hcl.j2
└── ...
```

## How it works

1. User fills the form in the Keystone Portal
2. Platform API reads `blueprints.json` to determine which modules are needed
3. Fetches `.j2` templates from this repo for the selected service type
4. Fetches scaffolding templates for `root-terragrunt.hcl`, `account.hcl`, etc.
5. Renders everything with Jinja2 using user-provided values
6. Pushes rendered HCL files to the team's infra repo → triggers CI/CD pipeline

## Adding a new service

1. Add a new entry to `blueprints.json`
2. Create a directory matching the service key with module subdirectories
3. Add `terragrunt.hcl.j2` templates for each module
4. The portal will automatically pick up the new service — no app redeploy needed
