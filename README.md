# Composer template for Drupal projects with Docker development environment

This project is an extension of [drupal-composer/drupal-project](https://github.com/drupal-composer/drupal-project) with the additional components of a ready to use Docker development environment and some preset configuration.


## Usage

To use this project you can use `composer create-project`

### Step 1: Create project

In this step we'll check out the Drupal site template into a folder, we'll
install all the utility tools like [Drush](https://www.drush.org/) and
[Drupal Console](https://drupalconsole.com/), set some default settings and
use some best practice in regards to configuration location.


#### Have composer installed?

If you have composer and PHP installed on you development host this step
is the easiest. Simply run

```
composer create-project --repository="{\"url\": \"git@github.com:thepearson/drupal-project.git\", \"type\": \"vcs\"}" --stability=dev --remove-vcs thepearson/drupal-project:8.x-dev [PROJECT_NAME]
```

Replace `[PROJECT_NAME]` with the actual name of your project.

#### Rather not?

If you don't have composer and PHP installed and you prefer to keep a clean
host machine. The following uses a composer/php Docker image to install
the template.

```
docker run --rm -it --volume ${COMPOSER_HOME:-$HOME/.composer}:/tmp --volume $(pwd):/app prooph/composer:7.3 create-project --repository="{\"url\": \"git@github.com:thepearson/drupal-project.git\", \"type\": \"vcs\"}" --stability=dev --remove-vcs thepearson/drupal-project:8.x-dev [PROJECT_NAME]
```

Fix permissions
```
chown -R `whoami`.`whoami` [PROJECT_NAME]`
```

### Step 2: Configure variables

#### Copy example .env file

```
cp .env.example .env
```

#### Set the project name

Al that's needed for development is to set the `PROJECT_NAME` variable.
Do this in the `.env` file.

### Step 3: Start the development environment

```
docker-compose up -d
```


### Step 4: Install Drupal

```
docker exec -it [PROJECT_NAME]-apache vendor/bin/drush -y site:install --site-name="My site name" --account-name=admin --account-pass="a complex password"
```

### Step 5: All done.

Now you can add the project to VCS. Go to [http://localhost:8080](http://localhost:8080) to see your new site.
