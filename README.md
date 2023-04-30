Download Link: https://assignmentchef.com/product/solved-cs300-homework-3-hash-table
<br>
In this homework we, as the manager of a grocery store, we ask you to develop a tool which will help us to improve our marketing strategy. We will provide you the purchased transactions of our customers as an input file and your tool needs to extract all the sales relations between the products. For this purpose, you are required to use a templated hashtable with separate chaining.OverviewWhen we go shopping, we generally have a standard list of things to purchase. This list is preparedbased on one’s needs and preferences for each customer. A housewife might purchase fresh food likevegetables or milk for the kitchen whereas someone alone might purchase chips and coke.Understanding these buying interests can offer assistance to extend deals in a few ways.For instance, if X and Y are frequently bought together:● X and Y can be both placed on the same shelf, so that buyers of one item would be prompted tobuy the other.● Promotional discounts could be applied to just one out of the two items (X or Y) since buyingone would encourage the buyer to buy the other one, too.● Advertisements on X could be targeted at buyers who purchase Y.The GoalThe goal of this homework, for a given list of transactions, is to extract all possible implications. Forinstance;chips -&gt; cokeThis implies that, for the customers who bought chips most likely bought coke, too. Or, as anotherexample, consider the following implication;sugar, milk -&gt; flourImplies that, the customers who bought sugar and milk most likely bought flour, too.Input File FormatYour program needs to ask two numbers (ranging from 0 and 1) for given support and confidencethreshold values (see their definitions in the document for details below) from the user. Then, yourprogram needs to ask the name of the input file. You can decide on the appropriate display messages forthem.The input file contains the list of purchased transactions of our customers. In each line, there are severalproducts (items) that are purchased for that time of shopping. For a transaction, there is no repeatedproduct and the number of products are not fixed. An example of input file is given as below:inputFile.txtmilk bread bananas cakebread apple onioncoke chips sandwichbananas yoghurt chocolate grapes orangemilk orange breadIn the input file, there are no empty input lines and all of the words are given as in lower cases. Thewords in the same line are separated with an empty space. An item will be only one word, so, you willnot see an item containing multiple words such as “orange juice”.Output File FormatThe output file format needs to be exactly the same as described here. You need to write your outputsto a file named as results.txt. In each line there will be only one rule. For each rule, you need to putcommas between the products of the same side of the implication and you need to put a semicoloninstead of the implication symbol (-&gt;). So, basically, what we expect from you is to generate the file asgiven in the second column of the following table if the extracted implications are as given in the firstcolumn of the table. There will be no space character in the output file. Moreover, you need to give theconfidence values of rules in the output file. Round your confidence values having 2 decimals, i.e.,having 2 digits after the dot as below. Continue reading this document for more details on confidencevalues.Extracted Rules with ConfidencesA -&gt; B conf = 0.30123{B, C} -&gt; D conf = 0.12139{B, C} -&gt; {A, D} conf = 0.32125Output file formatA;B;0.30B,C;D;0.12B,C;A,D;0.32Background InformationBefore going into detail with the algorithm, we need to define two concepts: support and confidence.Support describes how an item (or set of items) are popular among the whole item set. Hence, thesupport value of an item (or set of items) is the ratio of the number of times the item appeared amongall the transactions. For instance, if there are 10 transactions in the list and milk is purchased 4 times,the support value of milk is 0.4. On the other hand, if you need to calculate the support value of severalitems together, such as milk and bread, you need to find the number of transactions which contain bothmilk and bread together to find the support value of them.Confidence value describes how likely item Y is purchased when item X is purchased, expressed as X-&gt;Y.This is calculated as:confidence(X -&gt; Y) = support(X, Y) / support(X)For an implication having multiple items, such as {X, Y} -&gt; {Z, W}, confidence is calculated as:confidence({X, Y} -&gt; {Z, W}) = support(X, Y, Z, W) / support(X, Y)For instance; for the given support values;support(bread) = 0.5support(milk) = 0.4support(bread, milk) = 0.2The confidence of {bread -&gt; milk} is calculated as:confidence(bread -&gt; milk) = support(bread, milk) / support(bread)= 0.2 / 0.5 = 0.4Whereas the confidence of {milk -&gt; bread} becomes,confidence(milk-&gt;bread) = support(milk, bread) / support(milk)= 0.2 / 0.4 = 0.5The Flow of the ProgramThe program has 2 main parts: in the first part, you need to find the support value of several item setssuch as support({bread}) and support({bread, milk}). Then, by using those support values,you need to calculate the confidence values and extract the rules.Calculating Support Values● Find all support values of items in the transaction such as support({bread}) andsupport({milk}).● Store all the items whose support value is smaller than the given support threshold value.● Find all possible item pairs of the remaining items (selected after the previous step).● Find all support values of item pairs in the transaction such as support({bread, milk}),support({apple,orange}).● Store all the items pairs whose support value is smaller than the given support threshold value.Rule Extraction ProcessUp to this point, you have discovered all items and item pairs whose support values are greater than agiven support threshold value. Assume that all of these items and item pairs are stored in the same hashtable named as lookupTable.Using lookupTable (a hash table) you need to enumerate all possible 2 way permutations of all thelookupTable elements. For example, assume that in the hash table, we have the following elements:{A}, {B}, {C}, {A, B} and {B, C}You need to find all possible 2 length permutations of these elements and construct the rules as below:A -&gt; BA -&gt; CA -&gt; {B, C}B -&gt; AB -&gt; CC -&gt; AC -&gt; BC -&gt; {A, B}{A, B} -&gt; CHowever note that, there can not be the same item on both sides of the implication. For example, thefollowing rules are not valid.A -&gt; {A, B}{A, B} -&gt; {B, C}{A, B} -&gt; AAfter enumerating all possible rules, you need to calculate the confidence values of them. The output ofyour program will be the rules whose support values are greater the given confidence threshold value.Sample Runsin.txtmilk bread cakebread apple onioncoke chips sandwichbananas yoghurt chocolate grapes orangemilk orange breadyoghurt bananas juiceapple cucumber garlictea egg melonbread milk juice bananas orangemilk tea breadbread milk chocolatemilk coffeebroccoli bananas orange bread milkSample Run 1For the given input file above, the output will be:Please enter the transaction file name: in.txtPlease enter support and confidence values between 0 and 1: 0.20 0.6014 rules are written to results.txtresults.txtbananas;orange;0.75orange;bananas;0.75bread,orange;bananas;0.66orange,milk;bananas;0.66orange;bread;0.75bread;milk;0.85milk;bread;0.85bananas,orange;bread;0.66orange,milk;bread;1.00orange;milk;0.75orange;bread,milk;0.75bananas,orange;milk;0.66bread,orange;milk;1.00bananas,orange;bread,milk;0.66Sample Run 2For the given same input file above, the output will be:Please enter the transaction file name: in.txtPlease enter support and confidence values between 0 and 1: 0.60 0.45There is no rule for the given support and confidence values.