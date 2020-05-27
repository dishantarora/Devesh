package Test_pkg;

import org.testng.annotations.Test;

import org.testng.asserts.SoftAssert;

import org.testng.annotations.BeforeClass;

import org.testng.annotations.DataProvider;

import org.testng.annotations.Optional;

import org.testng.annotations.Parameters;

import static org.testng.Assert.assertEquals;

import java.io.File;

import java.io.FileInputStream;

import java.io.FileNotFoundException;

import java.io.FileOutputStream;

import java.io.IOException;

import java.util.List;

import java.util.Set;

import org.apache.log4j.BasicConfigurator;

import org.apache.log4j.Logger;

import org.apache.poi.ss.usermodel.Cell;

import org.apache.poi.ss.usermodel.Row;

import org.apache.poi.ss.usermodel.Workbook;

import org.apache.poi.util.SystemOutLogger;

import org.apache.poi.xssf.usermodel.XSSFCell;

import org.apache.poi.xssf.usermodel.XSSFRow;

import org.apache.poi.xssf.usermodel.XSSFSheet;

import org.apache.poi.xssf.usermodel.XSSFWorkbook;

import org.openqa.selenium.By;

import org.openqa.selenium.OutputType;

import org.openqa.selenium.TakesScreenshot;

import org.openqa.selenium.WebDriver;

import org.openqa.selenium.WebElement;

import org.openqa.selenium.chrome.ChromeDriver;

import org.openqa.selenium.firefox.FirefoxDriver;

import org.openqa.selenium.interactions.Actions;

import org.openqa.selenium.support.ui.Select;

import org.testng.Assert;

import org.testng.annotations.AfterClass;

public class Selenium_Test {

  WebDriver driver = null;

  String PageTitle = "omayo (QAFox.com)";

  static Logger logger = Logger.getLogger("Selenium_Test.class");

  @BeforeClass

  @Parameters({ "Browser", "URL" })

  public void setup(@Optional("Firefox") String browser, String url) {

    System.out.println("Inside Before class");

    switch (browser)

    {

      case "Chrome":

        System.setProperty("WebDriver.Chrome.Driver", "D:\\chromedriver.exe");

        driver = new ChromeDriver();

        break;

      case "Firefox":

        System.setProperty("webdriver.gecko.driver", "D:\\geckodriver.exe");

        driver = new FirefoxDriver();

        break;

      case "IE":

        break;

    }

    driver.get(url);

    driver.manage().window().maximize();

  }

  @Test(priority = -1, enabled = true)
  public void Logger() {
    BasicConfigurator.configure();
    logger.info("Welcome to advance selenium learning");
  }

  @Test(priority = -2, enabled = true)
  public void pageLoad() throws InterruptedException {
    System.out.println("Inside test class");
    String Title = driver.findElement(By.xpath("//div/h1[@class='title']")).getText();
    Assert.assertEquals(Title, "omayo (QAFox.com)");
    System.out.println("Page Title macth");

    // **Soft Assert**//
    SoftAssert softAssert = new SoftAssert();
    softAssert.assertEquals(Title, PageTitle);
    softAssert.assertAll();

    // **Text boxes operation using send keys**//

    driver.findElement(By.id("ta1")).clear();

    driver.findElement(By.id("ta1")).sendKeys("Hello testing using selenium");

    driver.findElement(By.xpath("//*[contains(text(),'The cat was playing in the garden.')]")).clear();

    driver.findElement(By.xpath("//*[contains(text(),'The cat was playing in the garden.')]"))
        .sendKeys("Hello testing using selenium");

    System.out.println("Value is entred");

    Thread.sleep(1000);

  }

  // **Data Provider **//
  @Test(dataProvider = "dataSet", priority = 1, enabled = false)
  public void dataProvider(String a1, String a2) throws InterruptedException

  {

    System.out.println("Inside data provider Method");

    driver.findElement(By.id("ta1")).sendKeys(a1);

    driver.findElement(By.xpath("//*[contains(text(),'The cat was playing in the garden.')]")).sendKeys(a2);

    System.out.println("Value is entred through data provider class");

    Thread.sleep(1000);

  }

  // **Drop Down**//
  @Test(priority = 3, enabled = false)
  public void dropDown()

  {

    WebElement drop = driver.findElement(By.id("drop1"));

    Select selectDrop = new Select(drop);

    selectDrop.selectByIndex(1);

    List<WebElement> dropOptions = selectDrop.getOptions();

    for (WebElement web : dropOptions)

    {

      System.out.println(web.getText());

      if (web.getText().equals("doc 1"))

      {

        System.out.println("Drop down value found");

      }

    }

  }

  // **Table on UI**//
  @Test(priority = 4, enabled = false)
  public void tableData()

  {

    System.out.println("Inside testDataOnUI() method priority =4");

    WebElement table = driver.findElement(By.id("table1"));

    List<WebElement> Rows = null;

    List<WebElement> Cols = null;

    int flag = 0;

    Rows = table.findElements(By.tagName("tr"));

    for (WebElement row : Rows)

    {

      System.out.println(row.getSize());

      if (flag == 0)

      {

        Cols = row.findElements(By.tagName("th"));

      }

      else

      {

        Cols = row.findElements(By.tagName("td"));

      }

      for (WebElement col : Cols) {

        System.out.print(col.getText());

      }

      flag++;

    }

  }

  @Test(priority = 5, enabled = false)
  public void testDataOnUI()
  {

    System.out.println("Inside testDataOnUI() method priority =g");

    WebElement table = driver.findElement(By.id("table1"));

    List<WebElement> Rows = null;

    List<WebElement> Cols = null;

    Rows = table.findElements(By.tagName("tr"));

    int flag = 0;

    for (WebElement row : Rows)

    {

      System.out.println(Rows.size());

      if (flag == 0)

      {

        Cols = row.findElements(By.tagName("th"));

      }

      else

      {

        Cols = row.findElements(By.tagName("td"));

      }

      for (WebElement col : Cols)

      {

        System.out.print(col.getText());

      }

      flag++;

    }

  }

  // **Reading an Excel File**//
  @SuppressWarnings("resource")
  @Test(priority = 6, enabled = false)
  public void readExcel() throws IOException, InterruptedException

  {

    System.out.println("Inside readExcel() method priority =6");

    File file = new File("D:\\Surinder.xlsx");

    // String filePath = System.getProperty("";

    FileInputStream fileStream = new FileInputStream(file);

    XSSFWorkbook workbook = new XSSFWorkbook(fileStream);

    XSSFSheet sheet = workbook.getSheet("sheet1");

    int numberOfRows = (sheet.getLastRowNum() + 1) - (sheet.getFirstRowNum());

    int numberOfCols = sheet.getRow(0).getLastCellNum();

    System.out.println("Total number of rows:" + numberOfRows);

    System.out.println("Total number of cols:" + numberOfCols);

    for (int i = 0; i < numberOfRows; i++)

    {

      driver.findElement(By.id("ta1")).clear();

      driver.findElement(By.id("ta1")).sendKeys(sheet.getRow(i).getCell(0).getStringCellValue());

      Thread.sleep(1000);

      driver.findElement(By.xpath("//*[contains(text(),'The cat was playing in the garden.')]")).clear();

      driver.findElement(By.xpath("//*[contains(text(),'The cat was playing in the garden.')]"))
          .sendKeys(sheet.getRow(i).getCell(1).getStringCellValue());

      System.out.println("Value is entred");

      Thread.sleep(1000);

    }

  }

  // **Frame handler/***
  @Test(priority = 7, enabled = false)
  public void frameHandle()

  {

    System.out.println("inside frameHandle() method priority= 7");

    driver.switchTo().frame("navbar-iframe");

    driver.findElement(By.id("b-query")).sendKeys("http://omayo.blogspot.com/");

    driver.switchTo().parentFrame();

  }

  // **Window handler**//
  @Test(priority = 8, enabled = false)
  public void windowHandle() throws InterruptedException

  {

    System.out.println("inside windowHandle() method priority= 8");

    String currentWindow = driver.getWindowHandle();

    driver.findElement(By.linkText("Open a popup window")).click();

    Set<String> windowHandles = driver.getWindowHandles();

    for (String string : windowHandles)

    {

      driver.switchTo().window(string);

      System.out.println(driver.getTitle());

      if (driver.getTitle().equals("Basic Web Page Title"))

      {

        System.out.println("On a valid window ");

        System.out.println(driver.findElement(By.id("para1")).getText());

      }

      else

      {

        System.out.println("No such window found");

      }

    }

    driver.switchTo().window(currentWindow);

    driver.findElement(By.id("ta1")).clear();

    driver.findElement(By.id("ta1")).sendKeys("Current window handles test case passed");

    Thread.sleep(1000);

  }

  @Test(priority = 9, enabled = false)
  public void excelWrite() throws IOException, InterruptedException

  {

    System.out.println("Inside excelWrite() method priority =9");

    File file = new File("D:\\OutputFile.xlsx");

    FileInputStream fileinputStream = new FileInputStream(file);

    XSSFWorkbook workbook = new XSSFWorkbook(fileinputStream);

    XSSFSheet sheet = workbook.createSheet();

    sheet.createRow(0).createCell(0).setCellValue("Hello");

    sheet.getRow(0).createCell(1).setCellValue("Surinder");

    sheet.getRow(0).createCell(2).setCellValue("Makkar");

    FileOutputStream filestream = new FileOutputStream(file);

    workbook.write(filestream);

    workbook.close();

    System.out.println("workbook operation is success");

  }

  // **Hover over**///
  @Test(priority = 10, enabled = false)
  public void actions()

  {

    System.out.println("Inside actions() method priority =10");

    driver.findElement(By.id("alert1")).click();

    driver.switchTo().alert().accept();

    System.out.println("alerts accepted");

    Actions alert = new Actions(driver);

    WebElement a1 = driver.findElement(By.id("blogsmenu"));

    WebElement a2 = driver.findElement(By.linkText("Selenium143"));

    alert.moveToElement(a1).moveToElement(a2).click().build().perform();

    System.out.println("actions successfull");

  }

  @Test(priority = 11, enabled = false)
  public void takeScreenshot()

  {

    System.out.println("Inside method takeScreenshot Priority =11");

    // TakesScreenshot scnshot = ((TakesScreenshot)driver);

    // File sourcefile = scnshot.getScreenshotAs(OutputType.FILE);

    // File destFile = new File ("D:\\Image.png");

    // FileUtils.CopyFile(sourcefile,destFile);

    // TakesScreenshot scnshot = ((TakesScreenshot)driver);

    // File sourcFile = scnshot.getScreenshotAs(OutputType.FILE);

    // File destFile = new File("D:\\Image.jpg");

    // FileUtils.CopyFiles(sourcFile,destFile);

  }

  @Test(priority = 0, enabled = false)
  public void excelReadWrite() throws IOException, InterruptedException

  {

    logger.info("Inside excelReadWrite method priority =0");

    WebElement drop = driver.findElement(By.id("drop1"));

    Select selectDrop = new Select(drop);

    List<WebElement> options = selectDrop.getOptions();

    logger.info("code after getting options list");

    File file = new File("D:\\OutputFile.xlsx");

    FileInputStream inputStream = new FileInputStream(file);

    XSSFWorkbook wb = new XSSFWorkbook(inputStream);

    XSSFSheet sh = wb.getSheet("Sheet1");

    int numberOfRows = (sh.getLastRowNum() + 1) - sh.getFirstRowNum();

    // wb.createSheet("Write");

    logger.info("number of rows :" + numberOfRows);

    for (int i = 0; i < numberOfRows; i++)

    {

      String sheetValue = sh.getRow(i).getCell(0).getStringCellValue();

      logger.info(sheetValue);

      for (WebElement web : options)

      {

        logger.info("checking sheet value");

        if (web.getText().equals(sheetValue))

        {

          logger.info("value is found");

          // selectDrop.selectByValue(web.getText());

          String text = driver.findElement(By.linkText("Page One")).getText();

          logger.info("value on UI :" + text);

          sh.createRow(numberOfRows + (i + 1)).createCell(0).setCellValue(text);

          FileOutputStream out = new FileOutputStream(file);

          wb.write(out);

          System.out.println("workbook operation is success");

          Thread.sleep(1000);

        }

      }

    }

    wb.close();

  }

  @Test(priority = 0, enabled = false)
  public void UItoExcelOperation() throws IOException

  {

    // File file = new File("D:\\UIData.xlsx");

    // FileInputStream inputStream = new FileInputStream(file);

    // XSSFWorkbook wb = new XSSFWorkbook(inputStream);

    // XSSFSheet sheet = wb.getSheet("Sheet1");

    WebElement table = driver.findElement(By.id("table1"));

    List<WebElement> Rows = table.findElements(By.tagName("tr"));

    List<WebElement> Cols = null;

    List<WebElement> Header = null;

    for (WebElement row : Rows)

    {

      logger.info(Rows.size());

      int flag = 0;

      if (flag == 0)

      {

        Header = row.findElements(By.tagName("th"));

        flag++;

        for (WebElement head : Header)

        {

          System.out.println(head.getText());

        }

      }

      else

      {

        Cols = row.findElements(By.tagName("td"));

      }

      for (WebElement col : Cols)

      {

        // driver.findElement(By.xpath("//tbody/tr/td[2][text()='25']"));

        System.out.println(col.getText());

      }

    }

  }

  @Test(priority = 0, enabled = false)
  public void readUITable() throws IOException

  {

    WebElement table = driver.findElement(By.id("table1"));

    List<WebElement> Rows = table.findElements(By.tagName("tr"));

    int rowCount = Rows.size();

    System.out.println("Row Size is:" + rowCount);

    List<WebElement> Cols = driver.findElements(By.xpath("//table[@id='table1']/tbody/tr[2]/td"));

    int colCount = Cols.size();

    System.out.println("Col Size is:" + colCount);

    File file = new File("D:\\ReadWrite.xlsx");

    FileInputStream inputStream = new FileInputStream(file);

    XSSFWorkbook wb = new XSSFWorkbook(inputStream);

    FileOutputStream out = new FileOutputStream(file);

    XSSFSheet sh = wb.createSheet("Table1");

    for (int i = 1; i <= rowCount; i++)

    {

      for (int j = 1; j <= colCount; j++)

      {

        if (i == 1)

        {

          System.out.println("inside header");

          String h1 = driver.findElement(By.xpath("//table[@id='table1']/thead/tr[" + i + "]" + "/th[" + j + "]"))
              .getText();

          System.out.println(h1);

          sh.createRow(i).createCell(j).setCellValue(h1);

        }

        else

        {

          System.out.println("inside data");

          String d1 = driver.findElement(By.xpath("//table[@id='table1']/tbody/tr[" + (i - 1) + "]" + "/td[" + j + "]"))
              .getText();

          System.out.println(d1);

          sh.createRow(i).createCell(j).setCellValue(d1);

        }

      }

    }

    wb.write(out);

    wb.close();

  }

  @Test(priority = 0, enabled = true)
  public void fileOperation() throws IOException, InterruptedException

  {

    File file = new File("D:\\Surinder.xlsx");

    FileInputStream inputStream = new FileInputStream(file);

    XSSFWorkbook wb = new XSSFWorkbook(inputStream);

    XSSFSheet sh = wb.getSheet("Sheet1");

    int noOfRows = (sh.getLastRowNum() + 1) - sh.getFirstRowNum();

    int noOfCol = sh.getRow(0).getLastCellNum();

    System.out.println("noOfRows:" + noOfRows);

    System.out.println("noOfCol: " + noOfCol);

    for (int i = 0; i < noOfRows; i++)

    {

      for (int j = 0; j < noOfCol; j++)

      {

        driver.findElement(By.id("ta1")).clear();

        driver.findElement(By.id("ta1")).sendKeys(sh.getRow(i).getCell(j).getStringCellValue());

        Thread.sleep(1000);

      }

    }

  }

  @AfterClass
  public void tearDown() {

    System.out.println("Inside after class");

    driver.quit();

  }

  @DataProvider
  public Object[][] dataSet() {

    Object[][] data = new Object[3][2];

    data[0][0] = "Hello 1";

    data[0][1] = "Hello 2";

    data[1][0] = "Hello 3";

    data[1][1] = "Hello 4";

    data[2][0] = "Hello 5";

    data[2][1] = "Hello 6";

    return data;

  }
}
