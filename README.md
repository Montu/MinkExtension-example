# A Behat 2.4 + Mink + 1.4 Demo

## Mink

Mink is a browser emulators abstraction layer.

It defines a basic API through which you can talk with specific browser emulator libraries.

Mink drivers define a bridge between Mink and those libraries.

Read [this article](http://knplabs.com/en/blog/one-mink-to-rule-them-all) to know more about Mink.

**This repository will allow you to easily try Mink and Behat to test… wikipedia.org!**

## Usage 

Clone this repo:

``` bash
git clone https://github.com/KnpLabs/mink-demo
```

Now install Behat, Mink, MinkExtension and their dependencies with [composer](http://getcomposer.org/):

``` bash
curl http://getcomposer.org/installer | php
php composer.phar install
```

Now to launch Behat, just run:

``` bash
bin/behat
```

Launch Behat: the two first scenarios should use Goutte.
The third one checks that the JS autocomplete field works on wikipedia: it uses Selenium WebDriver!
but lets ignore it for a quick start with `--tags` filter:

``` bash
vendor/bin/behat --tags ~@javascript
```

You should see an output like:

``` gherkin
Feature: Search
  In order to see a word definition
  As a website user
  I need to be able to search for a word

  Scenario: Searching for a page that does exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Behavior Driven Development"
    And I press "searchButton"
    Then I should see "agile software development"

  Scenario: Searching for a page that does NOT exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Glory Driven Development"
    And I press "searchButton"
    Then I should see "Search results"

2 scenarios (2 passed)
8 steps (8 passed)
```

### Selenium WebDriver

If you want to test `@javascript` part of feature, you'll need to install Selenium.
Selenium gives you ability to run `@javascript` tagged scenarios in real browser.

1. Download latest Selenium2 jar from the [http://seleniumhq.org/download/](Selenium website)
2. Run selenium jar with:

    ``` bash
    java -jar selenium.jar > /dev/null &
    ```

Now if you run:

``` bash
bin/behat
```

you should see an output like this:

``` gherkin
Feature: Search
  In order to see a word definition
  As a website user
  I need to be able to search for a word

  Scenario: Searching for a page that does exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Behavior Driven Development"
    And I press "searchButton"
    Then I should see "agile software development"

  Scenario: Searching for a page that does NOT exist
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Glory Driven Development"
    And I press "searchButton"
    Then I should see "Search results"

  @javascript
  Scenario: Searching for a page with autocompletion
    Given I am on /wiki/Main_Page
    When I fill in "search" with "Behavior Driv"
    And I wait for the suggestion box to appear
    Then I should see "Behavior Driven Development"

3 scenarios (3 passed)
12 steps (12 passed)
```
