package com.example.Tests;

import org.testng.annotations.Test;

import com.example.POClasses.ConfirmationPage;
import com.example.POClasses.HomePage;
import com.example.POClasses.RegisterPage;

import java.util.concurrent.TimeUnit;

import org.openqa.selenium.By;

import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.firefox.FirefoxDriver;

import org.openqa.selenium.ie.InternetExplorerDriver;
import org.testng.Assert;
import org.testng.annotations.AfterClass;

import org.testng.annotations.BeforeClass;
import org.testng.annotations.DataProvider;
import org.testng.annotations.Parameters;


public class Multibrowser {

	public WebDriver driver;

  @Parameters("browser")

  @BeforeClass

  public void beforeTest(String browser) {
  if(browser.equalsIgnoreCase("firefox")) {

	  driver = new FirefoxDriver();

  }else if (browser.equalsIgnoreCase("chrome")) { 

	  System.setProperty("webdriver.chrome.driver", "E:\\QA-Consultancy\\chromedriver.exe");

	  driver = new ChromeDriver();

  } 


  driver.get("http://newtours.demoaut.com/");
	driver.manage().timeouts().implicitlyWait(40,TimeUnit.SECONDS);
  }


  @DataProvider(name ="RegisterParameter")
  public Object[][] data(){
	  return new Object[][]{
		  {"ish","karthi","ish","ishaan","ishaan"},
		  {"test","sample1","idh","hdjs","hdjs"},
		  {"test2","sample2","dks","jfhkd","jfhkd"}
	  };
  }

  
 @Test(dataProvider="RegisterParameter")
 
 public void multipleuser(String first, String last,String email, String password, String confirmpassword)
 {
	 HomePage.lnk_Register(driver).click();
	  RegisterPage.getfirstname(driver).sendKeys(first);
	  RegisterPage.getlastname(driver).sendKeys(last);
	  RegisterPage.getemail(driver).sendKeys(email);
	  RegisterPage.getpassword(driver).sendKeys(password);
	  RegisterPage.getconfirmpassword(driver).sendKeys(confirmpassword);
	  RegisterPage.getsubmit(driver).click();
	  String expected = ConfirmationPage.getconfirm(driver).getText();
	  String actual = "Note: Your user name is " +email+ ".";
      System.out.println("actual:" +actual);
      System.out.println("expected:" +expected);
	  Assert.assertEquals(actual, expected);
     
	  }

  @AfterClass public void afterTest() {

		driver.quit();

	}

}





