# Random Wikipedia walker

Using Selenium, create a small program that, starting from the main page https://www.wikipedia.org/, walks trough a sequence of random links and takes a snapshot of the last page.
The process is as follows:

 1. Navigate to the main page https://www.wikipedia.org/
 2. Select a random link in the page
 3. Navigate to the link
 4. Repeat steps 2 to 3 until you have visited 10 different pages
 5. Take a snapshot of the current page and save it

Include the code of the walker and the snapshot in this document.

## Answer

```java
import java.io.File;
import java.util.List;
import java.util.Random;
import org.openqa.selenium.By;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.firefox.FirefoxDriver;
import org.openqa.selenium.io.FileHandler;

public class WikipediaWalker {
    public static void main(String[] args) {
        System.setProperty("webdriver.gecko.driver", "/chemin/vers/geckodriver");
        WebDriver driver = new FirefoxDriver();
        driver.get("https://www.wikipedia.org/");

        Random random = new Random();
        for (int i = 0; i < 10; i++) {
            List<WebElement> links = driver.findElements(By.cssSelector("#mp-topbanner a"));
            WebElement randomLink = links.get(random.nextInt(links.size()));

            randomLink.click();

            File screenshot = ((FirefoxDriver) driver).getScreenshotAs(OutputType.FILE);
            String filename = "capture" + (i + 1) + ".png";
            try {
                FileHandler.copy(screenshot, new File(filename));
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    driver.quit();
}
```