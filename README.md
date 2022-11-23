# Separate-and-Conquer-algorithm-for-pattern-set-composition

When using Monte-Carlo Tree Search algorithm to explore the search space, it will retrieve a pool of patterns as exlained in
[Link](https://github.com/MSc-MGomaa/MCTS-For-Rule-learning). For a dataset consists of N classes, N pools of patterns are generated, each of which is generated with respect to a particular class. Now, the goal is to use each pool of rules to create a corresponding ruleset, Which covers every example belongs to that class. In our implementation, Separate and Conquer (S&Q) algorithm is adopted.

## S&Q main phases
<p align="justify">
S&Q consists of two main stages, first, with a certain group of rules, find the pattern that best describes the current shape of the given dataset, while in the second stage (update stage), the samples covered by the pattern found are deleted from the dataset to create a new shaped one. In our implementation of the first stage of the S&Q, the goal was not only to find the rule that best describes the dataset, but also to know how much this rule contributes to the resulting pattern-set, and this can be achieved by knowing how many unique positive and negative samples each rule contributes to the composition of the pattern-set.

<p align="center">
<img width="800" height="450" src="https://github.com/MSc-MGomaa/Separate-and-Conquer-algorithm-for-pattern-set-composition/blob/87ab7db5004623f482f585a82600503f051c54b4/SQ.png">

### Finding the best pattern
<p align="justify">
In the figure above, under the assumption that the given dataset consists of only twp classes, positive and negative, and our aim is to compose the pattern-set that covers all the samples compose that class. With a given pool of patterns and a label, the pattern that best describes that label can be determined by evaluating all these patterns with respect to that label. This evaluation depends on the current shape of the dataset, as the dataset is updated on each iteration after finding
the best pattern, to produce a dataset with a different shape from the original one. In our implementation, M-estimate is used as an evaluation heuristic, taking into account the examples of negatives and positives covered by the current pattern. Once the best pattern is determined, it now remains to find how many unique negative and positive examples this rule covers. Here, the word unique is used because usually there is a covering cross (the sample could be covered by multiple patterns) between the patterns that make up the pattern-set. 
<p align="justify">
To handle that task, some steps have been adopted, it begins by finding the indices of the examples that make up the current label, Then a memory structure called coverSoFar is defined, which contains all the indices of the negative and positive samples covered by the current patterns
in the pattern-set. List negatives, to include the negative samples covered by the patterns of the pattern-set without any redundancies. List unCovered is defined as the difference between the indices of the samples that make up the current label, and those covered so far, this difference shows how many samples of the current label are still not covered by any of the current patterns in the pattern-set. Since our goal is to cover all the samples that make up the current label, a loop starts until all the positives are covered.
  
### Extent of the best pattern
<p align="justify">
Once the best patterb is found, the extent of that pattern is computed BUT w.r.t the original shape of the dataset, and not the updated version. All positive samples covered by this pattern can be calculated as an intersection between the extent of that pattern and the indices of positive samples in the dataset. The unique positive samples covered by this pattern can be found by comparing all the positive samples covered by that rule with the positive samples covered by all the patterns in the pattern-set so far. For negative samples, it is defined as the difference between the extent of the current pattern and the indices of the samples that make up the current label. The unique negative samples covered by this pattern can be found by comparing all the negative samples covered by that pattern with the negative samples covered by all the patterns in the pattern-set so far.
<p align="justify">
Once the unique positive and negative samples added by the current best best are identified, it remains to apply the update step to the dataset. To update the dataset, the extent of the current rule is calculated w.r.t the current shape of the dataset, the covered samples are dropped from the dataset to produce a new shaped dataset. The process is repeated until all positive samples make up the current label are covered.
  
### More Constraints
<p align="justify">
Since the end goal is to create a classifier with the resulting pattern-sets, the concept of generalization must be taken into account. Therefore, some constraints are taken into account after the creation of the pattern-set. First, if there is a rule ∈ the pattern-set that adds only one unique positive sample to the pattern-set, then that pattern will be dropped. Second, if the pattern covers more negatives than positives, then this rule is omitted. Finally, if the number of covered positives equals the negative samples covered, the pattern is also omitted.
  
## An Example for the pattern-sets found, one for each class for "IRIS" dataset:

<p align="center">
<img width="800" height="300" src="https://github.com/MSc-MGomaa/Separate-and-Conquer-algorithm-for-pattern-set-composition/blob/5c2ff6b9b227bc30ba79f9c4d27a79fe1907b4ab/result2.png">
  
From the results in the graph above, we can see that each class is represented with a set of patterns, NOT just single patterns as we did in [Link](https://github.com/MSc-MGomaa/MCTS-For-Rule-learning). Each set of patterns cover the sample space whcih represent each class, which we will use later to retrive a classifier.





  


