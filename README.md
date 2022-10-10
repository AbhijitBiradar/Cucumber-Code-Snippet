# Cucumber Code Snippet

# Syntax of sample feature file

```java 

Feature: LogIn Action Test

Description: This feature will test a LogIn and LogOut functionality

Scenario: Successful Login with Valid Credentials

Given User is on Home Page
When User Navigate to LogIn Page
And User enters UserName and Password
Then Message displayed Login Successfully

```

# Sample JUnit Runner Class Example

```java 

package cucumberTest;
import org.junit.runner.RunWith;
import cucumber.api.CucumberOptions;
import cucumber.api.junit.Cucumber;

@RunWith(Cucumber.class)
@CucumberOptions( features = "Feature",glue={"stepDefinition"})
public class TestRunner {
}

```

# Syntax of step Definition file

```java 

package stepDefinition;
import java.util.concurrent.TimeUnit;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.firefox.FirefoxDriver;
import cucumber.api.java.en.Given;
import cucumber.api.java.en.Then;
import cucumber.api.java.en.When;
public class Test_Steps {
    public static WebDriver driver;
    @Given("^User is on Home Page$")
    public void user_is_on_Home_Page() throws Throwable {
         driver = new FirefoxDriver();
         driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);
         driver.get("http://www.store.demoqa.com");
    }

    @When("^User Navigate to LogIn Page$")
    public void user_Navigate_to_LogIn_Page() throws Throwable {
         driver.findElement(By.xpath(".//*[@id='account']/a")).click();
    }

    @When("^User enters UserName and Password$")
    public void user_enters_UserName_and_Password() throws Throwable {
         driver.findElement(By.id("log")).sendKeys("testuser_1");
         driver.findElement(By.id("pwd")).sendKeys("Test@123");
         driver.findElement(By.id("login")).click();
    }

    @Then("^Message displayed Login Successfully$")
    public void message_displayed_Login_Successfully() throws Throwable {
         System.out.println("Login Successfully");
    }

    @When("^User LogOut from the Application$")
    public void user_LogOut_from_the_Application() throws Throwable {
         driver.findElement(By.xpath(".//*[@id='account_logout']/a")).click();
    }

    @Then("^Message displayed Logout Successfully$")
    public void message_displayed_Logout_Successfully() throws Throwable {
         System.out.println("LogOut Successfully");
    }
}

```

# Run Cucumber Tests in Groups with Cucumber Tags

```java 

@FunctionalTest
Feature: ECommerce Application

@SmokeTest @RegressionTest
Scenario: Successful Login
Given This is a blank test

In TestRunner mentioned tags as below
tags= {"@SmokeTest","@RegressionTest"}

```

# Ignore Cucumber Tests

```java 
Example: tags= {"~@SmokeTest","~@RegressionTest"}

```

# Logically ANDing and ORing Tags

```java
OR Syntax : Execute all tests tagged as @SmokeTest OR @RegressionTest
tags= {"@SmokeTest,@RegressionTest"}

And Syntax: Execute all tests tagged as @SmokeTest AND @RegressionTest
tags= {"@SmokeTest","@RegressionTest"}

```

# Hooks in Cucumber Test

```java 
public class Hooks {
    @Before
    public void beforeScenario(){
        System.out.println("This will run before the Scenario");
    }
    @After
    public void afterScenario(){
        System.out.println("This will run after the Scenario");
    }
}

```

# Tagged Hooks in Cucumber

```java 

@First
Scenario: This is First Scenario

Given this is the first step
When this is the second step
Then this is the third step

public class Hooks {
    @Before("@First")
    public void beforeFirst(){
        System.out.println("This will run only before the First Scenario");
    }
    @After("@First")
    public void afterFirst(){
        System.out.println("This will run only after the First Scenario");
    }
}

```

# Execution Order of Hooks

```java

public class Hooks {
    @Before(order=1)
    public void beforeScenario(){
        System.out.println("This will run before the every Scenario");
    }
    @After(order=0)
    public void afterScenarioFinish(){
        System.out.println("-----------------End of Scenario-----------------");
    }
    @After(order=1)
    public void afterScenario(){
        System.out.println("This will run after the every Scenario");
    }
}

```


# Background in Cucumber

```java

Background: User is Logged In
Given I navigate to the login page
When I submit username and password
Then I should be logged in

Scenario: Search a product and add the first product to the User basket

Given User search for Lenovo Laptop
When Add the first laptop that appears in the search result to the basket
Then User basket should display with added item


public class BackGround_Steps {
    @Given("^I navigate to the login page$")
    public void i_navigate_to_the_login_page() throws Throwable {
        System.out.println("I am at the LogIn Page");
    }
    @When("^I submit username and password$")
    public void i_submit_username_and_password() throws Throwable {
        System.out.println("I Submit my Username and Password");
    }
    @Then("^I should be logged in$")
    public void i_should_be_logged_in() throws Throwable {
        System.out.println("I am logged on to the website");
    }
    @Given("^User search for Lenovo Laptop$")
    public void user_searched_for_Lenovo_Laptop() throws Throwable {
        System.out.println("User searched for Lenovo Laptop");
    }
}

```

# Data Driven Test in Cucumber

1. **Data driven testing without Example Keyword**

```java 

Feature: Login Action

Scenario: Successful Login with Valid Credentials

Given User is on Home Page
When User Navigate to LogIn Page
And User enters "testuser_1" and "Test@123"
Then Message displayed Login Successfully


@When("^User enters \"(.*)\" and \"(.*)\"$")
    public void user_enters_UserName_and_Password(String username, String password) throws Throwable {
    driver.findElement(By.id("log")).sendKeys(username);
    driver.findElement(By.id("pwd")).sendKeys(password);
    driver.findElement(By.id("login")).click();
}

```


2. **Data driven testing using Example Keyword**

```java 

Feature: Login Action

Scenario Outline: Successful Login with Valid Credentials

Given User is on Home Page
When User Navigate to LogIn Page
And User enters "<username>" and "<password>"
Then Message displayed Login Successfully
Examples:
| username | password |
| testuser_1 | Test@153 |
| testuser_2 | Test@153 |


@When("^User enters \"(.*)\" and \"(.*)\"$")
    public void user_enters_UserName_and_Password(String username, String password) throws Throwable {
    driver.findElement(By.id("log")).sendKeys(username);
    driver.findElement(By.id("pwd")).sendKeys(password);
    driver.findElement(By.id("login")).click();
}

```

3. **Data driven testing using Data Table with List objects**

```java 

Scenario: Successful Login with Valid Credentials

Given User is on Home Page
When User Navigate to LogIn Page
And User enters Credentials to LogIn
| testuser_1 | Test@153 |
Then Message displayed Login Successfully


@When("^User enters Credentials to LogIn$")
    public void user_enters_testuser__and_Test(DataTable usercredentials) throws Throwable {
    //Write the code to handle Data Table
    List<List<String>> data = usercredentials.raw();
    //This is to get the first data of the set (First Row + First Column)
    driver.findElement(By.id("log")).sendKeys(data.get(0).get(0));
    //This is to get the first data of the set (First Row + Second Column)
    driver.findElement(By.id("pwd")).sendKeys(data.get(0).get(1));
    driver.findElement(By.id("login")).click();
}

```

4. **Data driven testing using Data Table with Maps objects**

```java 

Scenario: Successful Login with Valid Credentials

Given User is on Home Page
When User Navigate to LogIn Page
And User enters Credentials to LogIn
| Username | Password |
| testuser_1 | Test@153 |
| testuser_2 | Test@154 |
Then Message displayed Login Successfully

@When("^User enters Credentials to LogIn$")
    public void user_enters_testuser_and_Test(DataTable usercredentials) throws Throwable {
    //Write the code to handle Data Table
    List<Map<String,String>> data = usercredentials.asMaps(String.class,String.class);
    driver.findElement(By.id("log")).sendKeys(data.get(0).get("Username"));
    driver.findElement(By.id("pwd")).sendKeys(data.get(0).get("Password"));
    driver.findElement(By.id("login")).click();
}

```

5. **Data driven testing using Data Table with Class Objects**

```java 

Scenario: Successful Login with Valid Credentials

Given User is on Home Page
When User Navigate to LogIn Page
And User enters Credentials to LogIn
| Username | Password |
| testuser_1 | Test@153 |
| testuser_2 | Test@154 |
Then Message displayed Login Successfully


package stepDefinition;
public class Credentials {
    private String username;
    private String password;
    
    public String getUsername() {
        return username;
    }
    
    public String getPassword() {
        return password;
    }
}


@When("^User enters Credentials to LogIn$")
public void user_enters_testuser_and_Test(List<Credentials> usercredentials) throws Throwable {
    for (Credentials credentials : usercredentials) {
        driver.findElement(By.id("log")).sendKeys(credentials.getUsername());
        driver.findElement(By.id("pwd")).sendKeys(credentials.getPassword());
        driver.findElement(By.id("login")).click();
    }
}


```
