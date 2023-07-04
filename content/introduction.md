## Introduction
{:#introduction}
The internet is created and celebrated as a decentralized network that empowers individuals and fosters innovation in ways never thought imaginable. 
However, as we progress further into the digital age, the push for centralization of the internet has been significant. 
The control of the internet consolidates into the hands of a few massive companies.
These companies relentlessly collect, commercialize, and abuse this data in an unprecedented scale. 
The control of data exerted by these companies locks people into vendors, stifles competition in the online space, and raises pressing privacy concerns. \\
Luckily, a counter-movement to this trend has emerged in the (scientific) community.
A movement that increasingly pushes for the principles of decentralization on the web. 
One such example is the SOLID ecosystem—an innovative framework championed by visionaries like Sir Tim Berners-Lee, the creator of the World Wide Web. 
SOLID, which stands for "Social Linked Data," strives to empower individuals by giving them full control over their personal data. 
At its core, SOLID fosters a paradigm shift from the current model where data is locked away in centralized silos to a more inclusive and user-centric approach.
By decentralizing the web, SOLID allows users to store their personal information in secure, individual data pods while granting them granular control over who can access and manipulate their data. \\
Current querying approaches are not yet able to deal with the heavily decentralized data storage framework of SOLID.
Due to possibly millions of small data vaults being spread over the web, we need robust federated querying approaches that can efficiently retrieve the data vaults and execute queries over the retrieved data.
While federated query approaches exist CITE HERE, these assume that the possible sources are known beforehand and that there are only a few large sources. 
These assumptions do not hold for SOLID. 
Thus, we need a different querying approach that can deal with the high degree of decentralization. 
Enter Link Traversal-based Query Processing (LTQP) CITE HERE. In LTQP, the query engine starts with a set of seed documents and dynamically expands the number of possible source documents by following hyperlinks discovered in previously dereferenced documents. 
When querying over the entire Linked Data Web, the engine would in theory query every piece of linked data available, without specificying all possible sources on the web. 
However, this is also where the problem lies. 
Due to the lack of prior knowledge on the structure of data, the potentially infinite size of data accessed CITE HERE, and the numerous http request needed to obtain the data, LTQP is slow. CITE PAPER HERE. 
Even with reachability criterions that limit the accessed documents and various link prioritisation techniques CITE HERE, for the Linked Data Web LTQP is too slow for practical applications. 
However, when we turn our attention to the SOLID ecosystem, we find that the search space is significantly more restricted than the entire Linked Data Web, and that we have prior knowledge of the structure of the data stored. 
Thus, we hypothesise that we can leverage this prior knowledge to optimize the order in which we access links, and thus increase the throughput of LTQP in the SOLID ecosystem.
We will investigate this by introducing a simple priority queue in the query engine Comunica that prioritises links differently based on the way these links were discovered. If we are able to prioritise link sources that are likely more relevant to the query, we expect results to be completed more quickly, thus improving query throughput. 
Note that this approach likely does not improve total query execution time, because the number of links followed and query plan will stay the same CITE OLAF PAPER HERE.
<!-- After this section we will introduce our motivating example, then we will discuss our methodology and the used benchmark. Furthermore, we will discuss previous work on this topic, show our  -->