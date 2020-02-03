# Using the DNS Domain Trace view

When you create any DNS Domain Trace test, you get access to the DNS Domain Trace view.  The DNS Domain Trace view leverages the ThousandEyes standard layout, documented here: [https://support.thousandeyes.com/ViewArticle?articleIdParam=kA0E0000000CmmgKAC](https://success.thousandeyes.com/ViewArticle?articleIdParam=kA0E0000000CmmgKAC).  

This article highlights specifics shown in the DNS Domain Trace view.

## Example

This example shows the result of a globally distributed DNS Domain Trace test, targeting ultradns.net:A.

![Screen\_Shot\_2015-07-08\_at\_12.08.22\_AM.png](https://success.thousandeyes.com/servlet/rtaImage?eid=ka044000000UJk7&feoid=00NE0000006OT0r&refid=0EME0000000DWOw)

## View Specifics

The DNS Domain Trace test measures the following metrics:

* Availability: The percentage availability for the requested DNS record.
* Queries: The average number of queries sent to produce each trace.
* Final Query time: The average response time from the final \(authoritative\) DNS server.

For details on available metrics, access the knowledge base article here: [https://support.thousandeyes.com/ViewArticle?articleIdParam=kA0E0000000CmmzKAC](https://success.thousandeyes.com/ViewArticle?articleIdParam=kA0E0000000CmmzKAC)

The DNS Domain Trace view shows two different data views, depending on whether or not an Agent is selected, shown below:

<table>
  <thead>
    <tr>
      <th style="text-align:left">
        <p><b>Without a location selected</b>
        </p>
        <p>The computed averages area comprises of two sections</p>
        <p><b>Map: </b>computes the worldwide average for availability, number of
          queries, and final query time:</p>
        <p>
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka044000000UJk7&amp;feoid=00NE0000006OT0r&amp;refid=0EME0000000DWOv"
          alt="Screen_Shot_2015-07-08_at_12.11.40_AM.png" />
        </p>
        <p> <b>Table: </b>Tabulates for each Agent assigned to the test, &quot;Trace&quot;
          hyperlink, which shows full trace information, all mappings returned for
          the domain trace query, number of queries sent and final query time in
          ms.</p>
        <p>
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka044000000UJk7&amp;feoid=00NE0000006OT0r&amp;refid=0EME0000000DWPs"
          alt="Screen_Shot_2015-07-08_at_12.14.42_AM.png" />
        </p>
        <p>Without an Agent selected, the timeline shows the average availability
          of the domain, calculated across all Agents.
          <br />
          <br />
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka044000000UJk7&amp;feoid=00NE0000006OT0r&amp;refid=0EME0000000DWPv"
          alt="Screen_Shot_2015-07-08_at_12.12.05_AM.png" />
        </p>
      </th>
      <th style="text-align:left">
        <p><b>With a location selected</b>
        </p>
        <p>The computed averages area comprises of two sections</p>
        <p><b>Map: </b>shows whether the target domain is available from the Agent
          i.e OK or not i.e Error, number of queries, and final query time. Click
          on &quot;View Trace&quot; hyperlink for detailed trace data information
          ( similar to dig +trace &lt;domain_name&gt; from the command line)</p>
        <p>
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka044000000UJk7&amp;feoid=00NE0000006OT0r&amp;refid=0EME0000000DWOy"
          alt="Screen_Shot_2015-07-08_at_12.17.47_AM.png" />
        </p>
        <p><b>Table: </b>Tabulates for each Agent assigned to the test, &quot;Trace&quot;
          hyperlink, which shows full trace information, all mappings returned for
          the domain trace query, number of queries sent and final query time in
          ms. Selected Agent is highlighted in blue color.
          <br />
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka044000000UJk7&amp;feoid=00NE0000006OT0r&amp;refid=0EME0000000DWPq"
          alt="Screen_Shot_2015-07-08_at_12.24.49_AM.png" />
        </p>
        <p>The timeline plots the availability percentage of the target domain measured
          from the Agent as a line graph over the area graph showing the average
          availability percentage measured from all Agents assigned to the test.
          <br
          />
          <br />
          <img src="https://success.thousandeyes.com/servlet/rtaImage?eid=ka044000000UJk7&amp;feoid=00NE0000006OT0r&amp;refid=0EME0000000DWPn"
          alt="Screen_Shot_2015-07-08_at_12.19.41_AM.png" />
        </p>
      </th>
    </tr>
  </thead>
  <tbody></tbody>
</table>Note that there is no jump-to menu available for a DNS Domain Trace test, since network measurements cannot be enabled against many servers, due to the proliferation of anycast DNS services on the Internet.

