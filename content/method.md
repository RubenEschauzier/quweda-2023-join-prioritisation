## Method
{:#Method}

<span class="comment" data-author="RT">The approach definitely makes sense to me. However, I would suggest to present it in a different way (I'm aware that this is a draft atm). Currently, your "Method" section is actually more of an "Experimental Setup" section, since you talk about how to evaluate which priorities perform best. My suggestion would be to explain your approach/method (link sources, ...) in this "Method" section, and move all of the experimental setup things (5012 combinations for one query, best combinations for all queries, ...) to a new "Evaluation" section.</span>

While we expect that prioritizing links that are more likely relevant to the query result will improve the result arrival times, it is unclear which order of prioritization is optimal. 
Thus, we create link prioritization strategies in two ways. First, we determine an order and variations on that order that logically seem to make sense. 
Then, we enumerate all possible link priorities on a single query, record the priorities' performance characteristics and choose the priority order that is optimal on that single query. 
While this will give the optimal strategy for one particular query, there is no guarantee it will generalize to all other types of queries.
\\
Include more 'fluff' here about link sources
Possible link sources:

| Possible Link Sources                         | Short name |
|-----------------------------------------------|------------|
| Type Index                                    | Type       |
| http://www.w3.org/ns/ldp\#contains            | Contains   |
| http://www.w3.org/ns/pim/space\#storage       | Storage    |
| cMatch (Reachability criterion)               | cMatch     |
| http://www.w3.org/2000/01/rdf-schema\#seeAlso | SeeAlso    |
| http://www.w3.org/2002/07/owl\#\#sameAs       | SameAs     |
| http://xmlns.com/foaf/0.1/isPrimaryTopicOf    | TopicOf    |


### Combinations of Link Priorities
{:##Combinations of Link Priorities}
We expect links discovered using a Type Index to be highly relevant to the query. 
This is supported by CITE SOLID QUERY PAPER HERE. 
By default, comunica only follows type index links that match the triples in the query, thus it is likely that the contents of type index links will produce relevant data. 
Furthermore, given that we prioritize relevant sources, we expect that both the seeAlso and sameAs links will provide further relevant information due to these links referring to the same resource. 
Semantically, sameAs links will be closer to the links obtained from type indexes, thus we expect the sameAs link to be more relevant to the query than the seeAlso links.
The links obtained from Contains and Storage refer to .... 
The links from TopicOf ....
(Still have to figure out)
Finally, we have the links obtained by applying the cMatch criterion (CITE HERE). 
We expect this to have the lowest priority. In the LTQP literature, one flaw is the time it takes to execute queries. 
This is mainly due to the absence of prior knowledge of the structure of the Linked Data Web and its size. 
In this paper we expect to improve this by using prior knowledge of the structure of SOLID pods, thus any links that are not obtained from this structure likely have the lowest priority in our approach. 
Following the line of thinking in the previous paragraph, we construct the link priorities shown in [](#tab:priorities). 
We try all combinations of priorities between Contains, Storage, and TopicOf, because we are unsure of the effect prioritizing either of these link types will have on the query throughput. 

<figure id="tab:priorities" class="table" markdown="1">

|                 | Type | SameAs | SeeAlso | Contains | Storage | TopicOf | CMatch |
|-----------------|------|--------|---------|----------|---------|---------|--------|
| C1              | 1    | 2      | 3       | 4        | 5       | 6       | 7      |
| C2              | 1    | 2      | 3       | 5        | 4       | 6       | 7      |
| C3              | 1    | 2      | 3       | 6        | 4       | 5       | 7      |
| C4              | 1    | 2      | 3       | 6        | 5       | 4       | 7      |
| C5              | 1    | 2      | 3       | 5        | 6       | 4       | 7      |
| C6              | 1    | 2      | 3       | 4        | 6       | 5       | 7      |

<figcaption markdown="block">
The combinations of link priorities we will try in our experiment, each number denotes a priority in the queue, with 1 being the highest possible priority.
</figcaption>
</figure>

### Enumeration of Possible Link Priorities On a Single Query
{:##Enumeration of possible Link Priorities On a Single Query}
In addition to the link priorities obtained by reasoning over the semantics of the different SOLID link types, we also perform an optimization experiment on a single query of the SolidBench (CITE HERE) dataset (Still mentioned what query and why). 
In this experiment, we enumerate all 5012 combinations of priorities on a single query and use the link priorities that obtained the a first result in the least time, and the priority with the highest diefficiency metric (CITE HERE). 
Note that while these priorities will be the most performant on a singular query, we have no guarantee that this will generalize to all queries. 
The two priorities obtained from the enumeration experiment are given in [](#tab:prioritiesEnum).

<span class="comment" data-author="RT">These results seem very counter-intuitive. Are you sure they are correct? They seem to be the opposite to what we intuitively would expect. How large are the exec time differences? Perhaps the differences are too small (for this query)?</span>

<figure id="tab:prioritiesEnum" class="table" markdown="1">

|                 | Type | SameAs | SeeAlso | Contains | Storage | TopicOf | CMatch |
|-----------------|------|--------|---------|----------|---------|---------|--------|
| bestFirstResult | 7    | 6      | 4       | 5        | 1       | 2       | 3      |
| bestDiEff       | 5    | 3      | 1       | 6        | 2       | 4       | 7      |

<figcaption markdown="block">
The combinations of link priorities we will try in our experiment, each number denotes a priority in the queue, with 1 being the highest possible priority. We obtain the link priorities by running all combinations of priorities on a single query and recording the best combinations we find.
</figcaption>
</figure>
