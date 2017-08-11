---

copyright:
  years: 2015, 2017
lastupdated: "2017-08-09"

---

{:shortdesc: .shortdesc}
{:new_window: target="_blank"}
{:tip: .tip}
{:pre: .pre}
{:codeblock: .codeblock}
{:screen: .screen}
{:javascript: .ph data-hd-programlang='javascript'}
{:java: .ph data-hd-programlang='java'}
{:python: .ph data-hd-programlang='python'}
{:swift: .ph data-hd-programlang='swift'}

# Use cases
{: #usecases}

The {{site.data.keyword.tradeoffanalyticsshort}} service deals with the dilemma of how to make the best choice from among multiple options. Systematically comparing the available options, weighing all of the relevant pros and cons, and then confidently choosing the best possible option with minimal compromises is a difficult task. By employing formal methods and enabling visual and interactive representations, the service extracts and presents the best candidates and offers guidance and reasoning so that you will never miss a valuable option.
{: shortdesc}

The {{site.data.keyword.tradeoffanalyticsshort}} service can be used by applications in various domains to support users who are making decisions. The service can help with complex decisions, like what mortgage to take or which treatment to suggest for a patient, and with everyday choices, like which laptop to purchase. Businesses can transform the decision-making process into an actionable and productive undertaking, empowering their clients and employees while also meeting their expectations for control and independence.

The following sections suggest just a few of the potential applications of the {{site.data.keyword.tradeoffanalyticsshort}} service. Use this information as a catalyst for your own applications or to develop your own innovative ideas.

## Financial services and wealth management
{: #financial}

At some point, you have probably visited a web site that claims to help you make investment choices. Unfortunately, the tools that are prevalent on most investment sites invariably leave you overwhelmed. They typically present long lists of options with few filters and no real way to examine tradeoffs, making it likely that you will unintentionally ignore precious investment options and assets.

The {{site.data.keyword.tradeoffanalyticsshort}} service approaches this task differently. The service first lets you specify the criteria that are important to you, such as performance attributes, risk, and cost. The service then filters out investment options that are found to be inferior when evaluated against all of your criteria. This filtering reduces the problem space, letting you examine only the top investment options, those that meet or exceed your objectives. The service then helps you navigate the best candidates to see how they stack up and where small compromises can yield large gains.

Involving you, the investor, in this optimized process helps you choose the most suitable candidate and also minimizes regrets, complaints, and prosecutions. Similarly, bank analysts, financial advisors, and stock traders can use the {{site.data.keyword.tradeoffanalyticsshort}} service to select the best investment strategies based on their clients' individual objectives.

## Consumer goods
{: #consumer}

Imagine an e-commerce web site that sells appliances. Shoppers typically conduct endless searches, filtering and sorting the results in their quest to find the product that best matches their preferences based on attributes such as features, price, and warranty. By using the {{site.data.keyword.tradeoffanalyticsshort}} service, businesses can surface the most viable and valuable products immediately, highlighting the best deals for their customers. They can thus reduce customer churn and minimize transaction abandonment, which occurs when frustrated customers leave the site without making a purchase.

Application developers can extend this use case to any domain where products and services are to be compared and selected. For example, in a recent {{site.data.keyword.watson}} Hackathon, an application used the {{site.data.keyword.tradeoffanalyticsshort}} service to help users find the best area in which to live in San Francisco. The application allowed users to base the selection on lifestyle preferences such as proximity to a healthcare facility, air quality, asthma rate, proximity to a farmers' market, poverty rate, and affordability.

The travel domain offers another popular use case for the {{site.data.keyword.tradeoffanalyticsshort}} service in hackathons, where applications help users find a flight or accommodations, or even recommend vacation destinations. Other example applications address the selection of a person, such as the best candidate for a position or a sports team. A common thread that runs through all of these use cases is that the application goes beyond the filter-sort paradigm to offer a more powerful tool that has your back and will not let you miss out on a valuable option.

Each of these scenarios can be enhanced by combining {{site.data.keyword.tradeoffanalyticsshort}} with the {{site.data.keyword.personalityinsightsshort}} service to add a special dimension to the mix. Namely, product and service recommendations that consider the suitability of each option to the personality of the decision maker. For instance, the selection of a family vacation can take into account the personalities of the family members. By evaluating and considering personality characteristics along with more general attributes such as cost, destination, adventure level, and so on, an application can personalize the options that it recommends.

## Healthcare
{: #healthcare}

The {{site.data.keyword.tradeoffanalyticsshort}} service has many potential applications in the healthcare domain. Some apply to doctor-patient interaction and treatment selection, while others pertain to the development of medications and treatments. The following sections present two possible use cases.

### Selecting a treatment
{: #treatment}

Treatment selection is a difficult task, one in which no single option is best in every way or for every case. For example, imagine a physician who is considering treatment options for an oncology patient. The importance of making the right choice for the patient is rivaled only by the complexity of the decision.

Enter the {{site.data.keyword.tradeoffanalyticsshort}} service. The doctor can use the service to compare, together with the patient, the alternative treatments according to attributes such as rate of success, planned duration of the regimen, duration of effectiveness, level of adverse effects, and out of pocket expenses. The patient can even add criteria such as the distance to the clinic where the treatment is offered.

Such a joint decision can contribute to the success of the treatment in intangible ways. The patient is aware of the advantages and consequences of each option beforehand and is therefore likely to be even more committed to completing the course of treatment. Moreover, the transparent selection process, where the doctor discusses the pros and cons of each option with the patient, contributes positively to the chances of the patient's satisfaction with the selected treatment.

### Repurposing a drug
{: #drug}

Researchers at pharmaceutical companies are often interested in drug repurposing, where an existing, already-approved drug is used to treat a new condition. Consider for example a drug designer who is looking for possible treatments for rheumatoid arthritis, an immune system disease. The researcher's goal is to find and test existing drugs that are used for other diseases, drugs that may also have therapeutic effects for rheumatoid arthritis. The researcher looks for drugs that have biological activity that is similar to other arthritis medications.

Suppose also that the researcher knows that existing drugs inhibit a cell-signaling protein called TNF (tumor necrosis factor), which exacerbates autoimmune disorders such as rheumatoid arthritis. Of the many potential candidate drugs, the researcher needs to select only three for further clinical research. How can the researcher choose the best candidates?

The researcher turns to the {{site.data.keyword.tradeoffanalyticsshort}} service and specifies the objectives that matter most for this choice:

-   Intestinal absorption (the probability that a drug that is taken orally is absorbed into the blood)
-   Blood-brain-barrier penetration (the probability that the drug enters the brain)
-   Carcinogenicity (the probability that the drug causes cancer)
-   Rat acute toxicity (a measure called LD50 in which the drug is toxic to lab rats)
-   Not an hERG inhibitor (the probability that a side-effect of the drug can cause heart problems)

The {{site.data.keyword.tradeoffanalyticsshort}} service enables the researcher to focus only on the optimal drugs with respect to the specified objectives and to understand their tradeoffs. The researcher can then select from among the best candidate options based on the available data and objectives.
