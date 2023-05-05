<p align="center">
    <a href="https://cs.symfony.com">
        <img src="https://github.com/PHP-CS-Fixer/PHP-CS-Fixer/raw/master/logo.png" title="PHP CS Fixer" alt="PHP CS Fixer logo">
    </a>
</p>

PHP Coding Standards Fixer
==========================

The PHP Coding Standards Fixer (PHP CS Fixer) tool fixes your code to follow standards;
whether you want to follow PHP coding standards as defined in the PSR-1, PSR-2, etc.,
or other community driven ones like the Symfony one.
You can **also** define your (team's) style through configuration.

### Required in System

First, install pre-commit into your system. Python 3.8+ is required.

```console
pip install pre-commit
```

Then, make sure pre-commit has been installed correctly.
To confirm, run this command and if it return version number then it's good.

```console
pre-commit --version
```

## Installation

### 1. Install php-cs-fixer via composer
The recommended way to install PHP CS Fixer is to use [Composer](https://getcomposer.org/download/)
in a dedicated `composer.json` file in your project, for example in the
`tools/php-cs-fixer` directory:

```console
mkdir -p tools/php-cs-fixer
composer require --working-dir=tools/php-cs-fixer friendsofphp/php-cs-fixer
```

For more details and other installation methods, see [installation instructions](./doc/installation.rst).
This will create `./tools` folder in the root of your project.

### 2. Setup pre-commit config
Create new file in your project root folder and name it `.pre-commit-config.yaml`.
Inside of that file, put this configuration.

```yaml
repos:
- repo: local
  hooks:
    - id: php-cs-fixer
      name: php-cs-fixer
      entry: tools/php-cs-fixer/vendor/bin/php-cs-fixer fix --config=.php-cs-fixer.php --ansi --verbose --diff --dry-run
      language: script
      exclude: '^\.php-cs-fixer\.cache$'
- repo: https://github.com/pre-commit/pre-commit-hooks
  rev: v4.4.0
  hooks:
    - id: trailing-whitespace
    - id: end-of-file-fixer
```

This will use the installed `php-cs-fixer` in your local that is located in `./tools` folder.

```yaml
entry: tools/php-cs-fixer/vendor/bin/php-cs-fixer fix --config=.php-cs-fixer.php --ansi --verbose --diff --dry-run
```

### entry: breakdown

- `tools/php-cs-fixer/vendor/bin/php-cs-fixer`: This specifies the path to the PHP-CS-Fixer executable file.
- `fix`: This is the command to fix the code.
- `--config=.php-cs-fixer.php`: This specifies the path to the configuration file for PHP-CS-Fixer.
- `--ansi`: This enables the use of ANSI escape codes for colored output.
- `--verbose`: This enables verbose output, which includes more details about the changes made to the code.
- `--diff`: This generates a diff of the changes made to the code.
- `--dry-run`: This runs PHP-CS-Fixer without actually making any changes to the code.

Once you have a configuration file, you can apply it to the project by running.

```console
pre-commit install
```

## 3. Setup php-cs-fixer config
After setting up your `.pre-commit-config.yaml`, create new folder and name it `.php-cs-fixer.php` then insert this code.

```php
<?php

$finder = PhpCsFixer\Finder::create()
    ->exclude('vendor')
    ->in(__DIR__);

$config = new PhpCsFixer\Config();
return $config->setRules([
    '@PSR2' => true,
    'array_syntax' => ['syntax' => 'short'],
    'no_unused_imports' => true,
])
    ->setFinder($finder);
```

### Code explanation
This code is like a robot that checks your code to make sure it follows certain rules. It looks for things like unused imports and makes sure your code is written in a standard way. It also knows which files to check, and ignores some folders like "vendor". When you run this code, it will look at all the files you've changed, and if it finds any problems, it will tell and show you which file has the problem. Always add your files in staging area before running the `php-cs-fixer`, to add it in staging area just use `git add file_path`.


## 4. Update .gitignore
You don't want to push the contents of `./tools` and any cache that the php-cs-fixer produced.
Add this path inside of your `.gitignore` file.

```bash
/tools
.php-cs-fixer.cache
```

## Conclusion
In conclusion, this guide explains how to set up and use pre-commit hooks with PHP CS Fixer to ensure that your code follows coding standards. It requires installing pre-commit and PHP CS Fixer via Composer, setting up a pre-commit config file that specifies which files to check and which rules to follow, creating a PHP CS Fixer config file that defines coding standards, and updating the .gitignore file to exclude certain files and folders. Once set up, the pre-commit hook will check your code for errors and violations of coding standards, and if any are found, it will provide details on which files need fixing.

## How to use it
If you want to run the `php-cs-fixer` to check all the files in your staging area, just run this.

```console
git commit -v --no-edit
```

This command will run the `php-cs-fixer` and tells you the outcome of the test. This will only run the checker and will not commit your changes.

---

If you want to run the `php-cs-fixer` and at the same time commit your changes, just run this.

```console
git commit -m "Your commit message"
```

This command will run the `php-cs-fixer` and if there were no error it will automatically commit your changes. However, if there's an error it will abort commiting your changes and shows you the detailed information about the error.

---

If you don't want to run the `php-cs-fixer` and you only want to commit your changes, just run this.

```console
git commit -m "Your commit message" --no-verify
```

This command will not run the `php-cs-fixer` and immediately commit your changes.

## Troubleshooting
- If you encountered any error while setting up pre-commit, try running this commands:
  ```console
  pre-commit clean
  pre-commit install
  pre-commit autoupdate
  ```

## Screenshots
- This is an example of `php-cs-fixer` detecting an errors in your staging files.
![image](https://user-images.githubusercontent.com/104751512/236423214-6d183e1c-e0e4-4dfb-a8a9-b900386e4d8d.png)

- If there are no errors, this should be the output if you commit your changes.
![image](https://user-images.githubusercontent.com/104751512/236424256-4ac40ec2-f1b3-4539-9964-d1b676e246de.png)
