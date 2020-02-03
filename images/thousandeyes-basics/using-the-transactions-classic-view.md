# Using the Transactions \(Classic\) View



| Notice of obsolescence |
| :--- |
| This content is related to an older generation of ThousandEyes transaction test type, now renamed to **Transaction \(Classic\)**. We encourage you to start using the more powerful, JavaScript-based current generation of transactions. For more information about the current generation transaction testing, head over to the [Transaction Scripting Guide](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA044000000UFYvCAO_Transaction-Scripting-Guide). |

A Web Transaction test is a kind of page load test, where agents running the tests attempt to load a web page in a real browser, then interact with that page using a predefined script, written in Selenium. When you create any Web Transaction test, you get access to the Transactions View. The Transactions View leverages the [ThousandEyes standard layout](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA0E0000000CmmgKAC_ThousandEyes-view-layouts#standardlayout).

This article highlights specifics shown in the Web Transactions view.

## Example

The example below is based on the following script:

|  |  |  |  |  |
| :--- | :--- | :--- | :--- | :--- |
| 1. | Open LSE homepage | open | / |  |
| 2. | Enter "aapl" into search input box | typeKeys | xpath=//input\[@id="head\_solr\_search\_input"\] | aapl |
| 3. | Wait for the autosuggestion | waitForCondition | selenium.isElementPresent\('xpath=//li\[@class="ui-menu-item"\]/a/div/span\[text\(\)="Aapl"\]'\) && selenium.isVisible\('xpath=//li\[@class="ui-menu-item"\]/a/div/span\[text\(\)="Aapl"\]'\) | 5000 |
| 4. | Click the autosuggestion | click | xpath=//li\[@class="ui-menu-item"\]/a/div/span\[text\(\)="Aapl"\] |  |
| 5. |  | waitForPageToLoad |  | 15000 |

The screenshot below shows the result of a regionally distributed Web Transaction test to [https://www.londonstockexchange.com/](https://www.londonstockexchange.com/), searching for "AAPL" stock symbol and verifying that the resulting page loads.

![Transactions View](https://success.thousandeyes.com/servlet/rtaImage?eid=ka02R000000UOzs&feoid=00NE0000006OT0r&refid=0EM44000000EQH0)

Figure 1: Example transactions view

## View Specifics

Transaction tests offer a single View, the Transactions View. Below are the view-specific options.

#### Metrics

A given metric display changes, depending on whether an Agent location is selected.

* Completion
  * With no Agent selected: average percentage of total steps completed successfully.
  * With an Agent selected: percentage of steps completed successfully by that Agent. Each line of the transaction is numbered, starting from 0 \(opening the starting page\).
* Transaction Time
  * With no Agent selected: average time for transaction completion - only locations where the transaction completes successfully are used in this calculation.
  * With an Agent selected: the time required to complete the transaction, if completed successfully.

#### View Script

The View Script link displays the steps of the script. The contents of the Value field are not shown if they contain text, which could represent passwords or other sensitive information. Those fields will instead display asterisks \(\*\*\*\*\*\*\).

![View script popup](https://success.thousandeyes.com/servlet/rtaImage?eid=ka02R000000UOzs&feoid=00NE0000006OT0r&refid=0EM44000000EQIh)

Figure 2: View script popup

## Map Tab

#### Calculated Averages

Unlike other views, this section is pretty static. It includes the completion percentage and transaction time. As with other views, the data is either calculated using global averages, or from a single Agent location, depending on whether or not an Agent is selected.

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>Map tab of Transaction view with no location selected</b>
        </p>
        <p>
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka02R000000UOzs&amp;feoid=00NE0000006OT0r&amp;refid=0EM44000000EQJ6"
          alt="Details - all agents" />
        </p>
      </th>
      <th style="text-align:left">
        <p><b>Map tab of Transaction view with location selected</b>
        </p>
        <p>
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka02R000000UOzs&amp;feoid=00NE0000006OT0r&amp;refid=0EM44000000EQJB"
          alt="Details - one agent selected" />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>## Table Tab

The Table tab of a Transaction View displays a list of Agents, the date of the measurement in the user's chosen time zone, the completion of the test \(percentage, number of steps complete out of total steps\), and the transaction time for those steps.

![Table view of results](https://success.thousandeyes.com/servlet/rtaImage?eid=ka02R000000UOzs&feoid=00NE0000006OT0r&refid=0EM44000000EQJf)

Figure 5: Table view of results

## Timing Tab

The Timing tab of a Transaction View has two histogram graphics. The histogram on the left half of the page lists each transaction step by name and the time for each step. The histogram on the right half of the page lists each page by URL and the time for each page. Mouse over a histogram bar to display the timing information in a tooltip. Click on a histogram bar to switch to the Waterfall tab with the relevant page's waterfall displayed. Or click on a step or page name to make the switch to the Waterfall tab.

![Timing tab](https://success.thousandeyes.com/servlet/rtaImage?eid=ka02R000000UOzs&feoid=00NE0000006OT0r&refid=0EM44000000EQK9)

Figure 6: Timing tab

## Waterfall Tab

The Waterfall tab displays listings of objects loaded on each page and data associated with the loading of each object, along with data on the loading of the total page, for each page loaded. The itemized view and the list below describe the features on this tab.

![Waterfall tab](https://success.thousandeyes.com/servlet/rtaImage?eid=ka02R000000UOzs&feoid=00NE0000006OT0r&refid=0EM44000000EQKx)

Figure 7: Waterfall tab

1. **Page info:** The page title is shown, along with the URL of the page accessed.
2. **Page number banner:** To change the context of the waterfall to show a single page, click the page number in the banner. The size of the block for each page is proportional to the fraction of time spent on that page, relative to the total transaction time.
3. **Page boundaries:** Along with \#4, change the bounds to select the pages shown in the waterfall. Pull the left boundary to the right, and the right boundary to the left.
4. See \#3 above. In the example above, we're showing all four pages worth of data.
5. **Navigation buttons:** Use the Left, Right and "Show All" buttons to change the context of what is being shown. For example, where a single page is selected, the left and right buttons will change the selection to the previous and next page \(respectively\) in the transaction.  By default, the transaction will show each page visited starting with the start step and ending with the end step, as defined in the transaction script. If a single page is selected, that page's DOM and Page Load times will be shown as normal using the red and blue vertical bars transiting the waterfall. If the content being shown has multiple pages, the DOM and Page Load times will not be shown.

## Seeing it all in action

Ready for a more interactive approach? The screenshots included in this article were created from the transaction data contained in a public [share link](https://success.thousandeyes.com/PublicArticlePage?articleIdParam=kA0E0000000CmmyKAC_Sharing-Test-Data) available at the following URL: [https://pmttyk.share.thousandeyes.com](https://pmttyk.share.thousandeyes.com/).

