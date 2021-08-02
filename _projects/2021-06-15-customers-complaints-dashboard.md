---
title: 'Customers Complaints Dashboard'
subtitle: 'What do people talk about their financial institutions?'
featured_image: '/images/customers-complaints/background.jpg'
---

![](\images\customers-complaints\wall-5.jpeg)

## Abstract
---
In this project, I build the end-to-end data visualization and dashboard to display how customers complain about their financial institutions (banks, lenders, etc). The impact of this project is to provide a user-friendly interactive dashboard (Tableau and Streamlit) to visualize how customers complain about specific company and which problem the company faces. This web app consists of two main components: the **dashboard** and the **analysis** where the dashboard provides the interactive Tableau panel and the analysis shows polar graphs of customer complaints associated with the company being criticized.

## Design
---
- The Covid-19 pandemic has affected everyone!
- We've all witnessed a tremendous change in our lives since the pandemic happened. Even though businesses are gradually reopening, the national economics has suffered huge pain from it.
- How do people feel about their financial institutions (banks, lenders, financial companies, etc) treating them during the pandemic?
- In this project, I will use the customers complaint database from [Consumer Financial Protection Bureau](https://www.consumerfinance.gov/) to do sentiment analysis and exploratory analysis to see the trend of customers' satisfaction over time.

## Data
---
- The data is obtained by pulling from [CFPB API](https://cfpb.github.io/api/ccdb/api.html).
- The final dataset has over **1M** data points and being updated weekly.

Here is the example of how the data is store in **json** format:
```
{'tags': 'Older American',
 'zip_code': '98198',
 'complaint_id': '2701967',
 'issue': 'Incorrect information on your report',
 'date_received': '2017-10-14T12:00:00-05:00',
 'state': 'WA',
 'consumer_disputed': 'N/A',
 'product': 'Credit reporting, credit repair services, or other personal consumer reports',
 'has_narrative': True,
 'company_response': 'Closed with non-monetary relief',
 'company': 'EQUIFAX, INC.',
 'submitted_via': 'Web',
 'date_sent_to_company': '2017-10-14T12:00:00-05:00',
 'company_public_response': None,
 'sub_product': 'Credit reporting',
 'timely': 'Yes',
 'complaint_what_happened': 'XXXX currently known as XXXX has reported a hard inquiry by me this is not my inquiry I have had the same account for over 30 years I do not live in XXXX it is not my inquiry needs to be removed',
 'sub_issue': 'Information belongs to someone else',
 'consumer_consent_provided': 'Consent provided'}
```
## Tools
---
- Python
- Pandas
- Numpy
- NLTK
- MongoDB
- Streamlit
- Plotly

## Methodology

### Workflow

- API calling from [CFPB API](https://cfpb.github.io/api/ccdb/api.html) weekly to update the dashboard.
- Store the data in MongoDB and have it ready for processing and building dashboard.
- Tableau and Plotly for visualization
- NLTK for sentiment analysis.
- Streamlit for deploying.

![](\images\customers-complaints\data_engineering_workflow.png)

### Topic Modeling

- Topic Modeling using TF-IDF and NMF: 5 topics include

```
    - inaccurate credit report
    - mortgage and late payments
    - deb collection
    - dispute credit card charge
    - identity theft
```

![](\images\customers-complaints\topics.png)

### Building Dashboard

#### Homepage

This web app consists of two main components: the **dashboard** and the **analysis**.
- **The dashboard**: the dashboard is powered by [Tableau Public](https://public.tableau.com/s/) and embedded into streamlit. The data is being used consists of over 1M data points and is updated weekly. We can easily interact with it on the fly.
- **The analysis**: the full 1M customer complaints were encoded using *Term Frequency–Inverse Document Frequency (TF-IDF)* and used to build *Non-negative Matrix Factorization (NMF)* topic modeling. Upon topics extracted from the model, plotly polar graphs are being used to show the diversity of complaints and the company associated with.

![](\images\customers-complaints\homepage.png)

#### Dashboard

- The dashboard is built and powered by [Tableau Public](https://public.tableau.com/s/) and embeded to the web app using HTML and JS tags.

![](\images\customers-complaints\dashboard1.png)

- We can easily interact and filter with the dashboard inside the web app on the fly. In this case, we see that Texas alone has total of 23,239 complaints in the past year. The top products are credit reporting, debt, checking account, etc. The top companies that get complained are Transunion Intermediate Holding, Experian Information Solution, Equifax, etc.

![](\images\customers-complaints\texas-1yr.png)

#### Analysis

- The anlysis is built using 5 topics extracted from topic modeling and polar plotted using Plotly.
- In this case, from 03/01/2020 to 2021/07/07, AMA Advisors, LLC have mostly their complained being classified as mortgage and late payments and dispute credit card charge. There are few complaints associated with other topics, but mostly these two topics.

![](\images\customers-complaints\analysis1.png)

- We can also see 3 random complaints about this particular institution by clicking the + symbol. For example, on 11/09 last year, one customer said that *"I've been trying ever since XXXX the XXXX to get my mortgage documents. Homebridge Mortgage keeps telling me their sending it out and that I will receive it within 72 hours but I've never received any documents or papers since XXXX the XXXX. I've called 6 times within that time frame. Ive asked to speak with the CEO to get this resolved, they gave me someone in office and they stated the same info about sending within 72 hours. Still I have nothing! Please help me get this resolved asap, thank you for your service!"*. He/she sounds really frustrated.

![](\images\customers-complaints\analysis2.png)

---
**Thank you for your time!** If you're interested in this project, you can see the source code [here](https://github.com/luongtruong77/data-engineering-customers-complaints-dashboard) on Github, or you can reach out to me [here](https://www.linkedin.com/in/luongtruong77/) on LinkedIn.