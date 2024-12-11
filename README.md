# QuIC GitHub Organization

This repo contains workflow templates and other org level config and files

## QuIC Repolinter GitHub Action

QuIC repos should enable the Repolinter GitHub Action upon creation. This action runs the Repolinter tool, which lints open source repositories for common issues.

### Initial setup

1. Navigate to the new repo
1. Click on Actions
1. If you have existing actions in the repo, click "New workflow", else skip to next step
1. Locate the "By Qualcomm Innovation Center" section and click "Configure" under "QuIC Organization Repolinter Workflow"
1. Click "Commit changes...", select "Commit directly to the main branch" (or feel free to create a new branch and start a PR), ensure your QuIC email is selected under "Commit Email", and then click "Sign off and commit changes"
1. This will create a GitHub Action config file in your repo under the path `.github/workflows/quic-organization-repolinter.yml`
1. Adjust it as needed, e.g. the Repolinter action is configured to run on Push and Pull Requests into the main/master branch, but you may want to further adjust when it runs.

### Customizing Repolinter rules

When the GitHub Action is run, it first checks your QuIC repo for a local `repolint.json` file at the root directory. If it doesn't find one it'll use the default QuIC Repolinter ruleset, which is located here https://github.com/quic/.github/blob/main/repolint.json

To customize the default QuIC repolinter ruleset (e.g. to add some language specific file extensions for the license check), you can extend the default ruleset and override specific rules. 

For example, if we wanted to exclude the copyright/license check for a directory e.g. `/test-data` from Repolinter:

1. Create the file `repolint.json` at the root of your project
1. "Extend" the QuIC repolint.json file

```json
{
  "extends": "https://raw.githubusercontent.com/quic/.github/main/repolint.json",
  "rules": {}
}
```

3. Copy the rule block you need to adjust from `https://raw.githubusercontent.com/quic/.github/main/repolint.json`. E.g. in this case we want to exclude `/test-data` from the license header check. So let's copy the json block `source-license-headers-exist` and paste it into the `rules` section of the local `repolint.json` we extended
1. Now lets add the `test-data` directory to the list of patterns to skip/exclude from being checked

```json
{
  "extends": "https://raw.githubusercontent.com/quic/.github/main/repolint.json",
  "rules": {
    "source-license-headers-exist": {
      "level": "error",
      "rule": {
        "type": "file-starts-with",
        "options": {
          "globsAll": [
            "**/*.py",
            "**/*.js",
            "**/*.c",
            "**/*.cc",
            "**/*.cpp",
            "**/*.h",
            "**/*.ts",
            "**/*.sh",
            "**/*.rs",
            "**/*.java",
            "**/*.go",
            "**/*.bbclass",
            "**/*.S"
          ],
          "skip-paths-matching": {
            "patterns": [
              "babel.config.js",
              "build\/",
              "jest.config.js",
              "node_modules\/",
              "types\/",
              "uthash.h",
              "test-data\/"
            ]
          },
          "lineCount": 60,
          "patterns": [
            "(Copyright|©).*Qualcomm Innovation Center, Inc|Copyright (\\(c\\)|©) (20(1[2-9]|2[0-2])(-|,|\\s)*)+ The Linux Foundation",
            "SPDX-License-Identifier|Redistribution and use in source and binary forms, with or without"
          ],
          "flags": "i"
        }
      }
    }
  }
}
```

# Creating a Whitelist for Copyright/License Check in Repolinter
  * To include only specific directories (e.g., `/test1-data`, `/test2-data`) in the copyright/license check using Repolinter, follow these steps:

For example, if we wanted to create a whiltelist the copyright/license check for list of directory (e.g. `/test1-data`, `/test2-data`) from Repolinter:

1. Create the file `repolint.json` at the root of your project
2. "Extend" the QuIC repolint.json file

```json
{
  "extends": "https://raw.githubusercontent.com/quic/.github/main/repolint.json",
  "rules": {}
}
```

3. Copy the rule block you need to adjust from `https://raw.githubusercontent.com/quic/.github/main/repolint.json`. E.g. in this case we want to include only `/test1-data`, `/test2-data` from the license header check. So let's copy the json block `source-license-headers-exist` and paste it into the `rules` section of the local `repolint.json` we extended.
4. Adjust the rule to include only the (e.g. `/test1-data`, `/test2-data`) directories.
   
* Your repolint.json should look like this:

```json
{
  {
  "extends": "https://raw.githubusercontent.com/quic/.github/main/repolint.json",
  "rules": {
    "source-license-headers-exist": {
      "level": "error",
      "rule": {
        "type": "file-starts-with",
        "options": {
          "globsAll": [
            "test1-data/**",
            "test2-data/**"
          ],
          "lineCount": 60,
          "patterns": [
            "(Copyright|©).*Qualcomm Innovation Center, Inc|Copyright (\\(c\\)|©) (20(1[2-9]|2[0-2])(-|,|\\s)*)+ The Linux Foundation",
            "SPDX-License-Identifier|Redistribution and use in source and binary forms, with or without"
          ],
          "flags": "i",
          "succeed-on-non-existent": true
        }
      }
    }
  }
}
}
