# Author Response

We express our sincere gratitude to all three reviewers for their invaluable input and constructive comments on our paper.

We have diligently revised our paper in response to the suggestions provided by the Associate Editor and the three reviewers. Here, we present our response to the list of major changes requested by the Associate Editor and each individual reviewer.

```
From Associate Editor Anders Møller:

Recommendation #1: Major Revision

The reviewers appreciate your revision but still have some concerns.

Specifically, some central claims and definitions are still misleading and too vague. Since some call graph must be pre-computed, this analysis is actually less precise than standard k-CFA formulations with on-the-fly call graph, since its results may be influenced by code that the analysis would find to be unreachable.

The paper claims "the first CFL-reachability formulation of k-CFA", yet it is not used for k-CFA analysis, but rather as a pre-analysis. If the approach cannot entirely replace k-CFA, then it doesn’t make sense to call it "a formulation of k-CFA".  The role of pre-analysis needs a clarification, or the paper's contributions need the reformulated. For the second revision, these concerns must be addressed. Please see the reviews for details.
```
**Authors:** 
The $L_{DCR}$-based pointer analysis is equivalent to $k$-CFA. It incorporates on-the-fly call graph construction, leveraging the initial call graph constructed using CHA in a linear manner. The initial call graph used
won't affect precision, as the $L_{DCR}$-based pointer analysis will effectively filter out any spurious target methods, as demonstrated through the use of our motivating example and further explained in Lines 1013-1017.

Demand-driven and whole-program pointer analyses are recognized as equivalent (by the program analysis community), differing only in how the points-to information is computed. Consequently, $L_{DCR}$ and $k$-CFA share this equivalence, as they follow the same distinction between the two analysis approaches.

We have addressed the equivalence and disparity between demand-driven and whole-program pointer analyses by introducing a new paragraph in Lines 1018-1026. Furthermore, to enhance the understanding, we have included a new example in Figure 11, which serves as an illustration of this relationship.

In addition, we have also added a new paragraph to Section 4.1, providing background information on selective context sensitivity and the significance of
applying $L_{DCR}$ for performing a pre-analysis to accelerate $k$CFA while preserving its precision.

It is worth noting that each CFL-reachability formulation is developed based on an underlying graph, e.g., PAG. When we say $L_{DCR}$ is a complete specification of $k$-CFA, we mean that it can dynamically construct the call graph on-the-fly directly from the provided input PAG. This eliminates the need for an external algorithm, such as the one used by $L_{FC}$, to build the call graph. Thus, $L_{DCR}$ is considered complete in the sense that it incorporates call graph construction within its formulation, while $L_{FC}$ does not. The specific method of constructing the PAG for $L_{DCR}$ is irrelevant in this context as long as it is sound. For instance, instead of using CHA, context-insensitive pointer analysis can also be employed to build the initial call graph. Nevertheless, we have revised our statement to  describe our formulation as a specification of callsite-sensitive pointer analysis with built-in on-the-fly callgraph construction. We have made the necessary adjustments throughout the paper by removing the terms "complete specification" and "incomplete".

# Referee: 1

```
Comments to the Author
The paper has improved in presentation as well as experimental evaluation following previous reviews. In particular, the experimental section now compares the efficiency and precision of k-CFA using the proposed method as a pre-analysis, to two other pre-analyses in the literature. The results show that the newly proposed pre-analysis can sometimes strike a better balance of precision and running time, though the improvements are mild most of the time. All in all, this is a nice improvement, but my main issues remain. I find the contributions to be not yet clear enough for appearance in TOPLAS. In particular:

- line 85 "Theoretically, we therefore demonstrate for the first time that callsite-sensitive pointer analysis is a special kind of context-sensitive language that can be expressed as the intersection of several CFLs". These kind of claims remain formally incorrect. The proposed analysis is Turing complete.
```
**Authors:**  
We have revised the sentences on Page 2 (lines 88-92) to accurately reflect the nature of our $L_{DCR}$ language. As mentioned in the last paragraph of Liu's paper [34] and illustrated in a figure on page 20 of Latta's PhD thesis [27], there exists a context-sensitive language, such as $L = {a^{2^n}\mid n \in \mathbb{N}}$, that cannot be expressed as the intersection of a finite number of CFLs. In contrast, we demonstrate that $L_{DCR}$ can be expressed as the intersection of three CFLs, which provides a higher level of precision than simply stating that $L_{DCR}$ is a Turing-complete language. We have also included the appropriate citations for these references. Thank you for helping us clarify this.


```
- As the paper states in line 1037, the proposed analysis can be made a decidable by reducing to k-limiting paths, as per standard in the literature. Would it then be sound as precise as k-CFA? If so, this would be a very welcome theoretical claim to make and strengthen the paper.
```
**Authors:** 
We would like to emphasize that the primary contribution of this paper is the introduction of a CFL-reachability formulation expressed as the intersection of three CFLs for callsite-sensitive pointer analysis with built-in on-the-fly callgraph construction. Our intention is not to propose a new formulation to completely replace $k$-CFA in terms of computing points-to information. We
have mentioned some potential applications of our formulation in the first
paragraph on Page 3. To demonstrate its significance in this paper, we have developed a pre-analysis technique based on our CFL-reachability formulation to enhance the efficiency of $k$-CFA without sacrificing precision.

While it is possible to make our CFL-reachability formulation decidable by leveraging $k$-limiting techniques, the $L_{DCR}$-based pointer analysis and $k$-CFA compute the points-to information in a program in an equivalent yet distinct manner.

To address the equivalence and disparity between $L_{DCR}$-based demand-driven and whole-program pointer analyses, we have introduced a new paragraph in Lines 1018-1026. Moreover, to further illustrate this concept, we have included a new example in Figure 11. These additions aim to enhance the understanding of the relationship between the two analysis approaches.


```
- Given the above claim, why is the proposed method not used and evaluated as an alternative to k-CFA? Why is it used only as a pre-analysis?
```
**Authors:** The main objective of this paper is not to propose an alternative approach to replace $k$-CFA, but rather to introduce a CFL-based formulation known as $L_{DCR}$ for call-site sensitive pointer analysis, as illustrated in Figure 1. A significant application of $L_{DCR}$ demonstrated in this paper is its ability to facilitate the development of a precision-preserving pre-analysis technique known as P3Ctx, which aims to accelerate the performance of $k$-CFA while preserving its precision.

As discussed in the recently added Lines 1018-1026, it is worth noting that $L_{DCR}$ is particularly well-suited for implementing demand-driven pointer analyses, which are distinct from the whole-program analyses represented by $k$-CFA. To further emphasize the equivalence and disparity between these two approaches, we have included a new example in Figure 11. This example is carefully explained in the newly added Lines 1046-1063, providing a detailed illustration of the similarities and differences between demand-driven and whole-program analyses (known by the program analysis community).

This paper makes a theoretical contribution by introducing $L_{DCR}$, while P3Ctx serves as a practical contribution. Future work will focus on exploring additional practical applications of $L_{DCR}$.

```
- line 638. This "Definition" remains way to vague to amount to a formal definition in a PL paper. This vagueness also pollutes later formal statements (Lemma 2, Theorem 1) that make use of terminology set up in this definition.
```
**Authors:** 
We have now included a new paragraph just before Definition 1. Since the primary focus of this paper is the development of a new CFL-based formulation for call-site sensitive pointer analysis, our formalism is similar to that employed in existing literature \cite{sridharan2005demand, sridharan2006refinement, zheng2008demand, yan2011demand, shang2012demand}. We hope this clarification is satisfactory. Thanks.

```
- Section 4. It is important to state what a pre-analysis for k-CFA is and what its properties are before constructing one. The term "pre-analysis" is not given a meaning throughout the paper. One aspect unclear to me is why the various pre-analyses evaluated in Section 5 impact the precision of the main analysis, as opposed to only its performance (since the main analysis is precise).
```
**Authors:** 
We have made an addition to Section 4, introducing a new paragraph at the beginning, to provide background information on selective context sensitivity and to clarify the role of a pre-analysis technique in supporting selective pointer analysis. This addition aims to enhance the readers' understanding of the context and significance of the subsequent discussions on selective pointer analysis.


```
- Since the authors acknowledge similarities with Sridharan's approach [51] in both the goals and the techniques of this paper, it is unclear why that method is not experimentally evaluated as well.
```
**Authors:** We did not compare our formulation with Sridharan's approach because we were unaware of Sridharan's work, as it has not been published in a peer-reviewed publication. The reference to Sridharan's approach was added to our paper based on the request of the third reviewer during the first major revision. It is important to note that our formulation and Sridharan's approach share the same underlying objective of addressing the same theoretical problem of formulating call-site sensitive pointer analysis with on-the-fly call graph construction using the intersection of several CFLs. However, there are fundamental differences between our approach and Sridharan's, which are summarized in Section 5.1. Furthermore, it is not possible to compare P3Ctx with any pre-analysis technique based on Sridharan's approach as no such pre-analysis technique exists.
  

# Referee: 2
```
Comments to the Author
I appreciate the authors' response and revision. However, I still have some major concerns.

The authors argued that the new CFL-reachability formulation is “complete”, since an initial call graph (obtained by CHA) is somehow inevitable for CFL-reachability (while I don’t fully agree with this), and CHA can be done efficiently. However, I think the phrase “a complete specification for call graph construction” is still very misleading and might give the readers wrong impressions. A genuine “complete specification” should consist of two steps: (1) building some kind of graph G from the code (G doesn’t need to be PAG), and (2) performing “call graph construction” by doing CFL-reachability on G. It’s important to ensure that step (1) doesn’t contain any “call graph construction” ingredients, otherwise the subsequent CFL-reachability can only be regarded as a “call graph refinement” instead of “a complete specification for call graph construction”. However, in the current paper, step (1) already contains CHA-based call graph construction, so I don’t think the subsequent CFL-reachability is really a “complete” call graph construction specification. As a result, the incomplete specification is not very useful in places like analyzing the hardness of the graph construction problem (since it doesn’t completely represent the problem), while a genuine complete specification should be useful in those places.
```
**Authors:** Our formulation utilizes a PAG that relies on an initial call graph. This call graph can be constructed using CHA, which leverages solely the type information of the program. The advantage of CHA is that it enables the linear-time construction of the initial call graph.

It is worth noting that each CFL-reachability formulation is developed based on an underlying graph, e.g., PAG. When we say $L_{DCR}$ is a complete specification of $k$-CFA, we mean that it can dynamically construct the call graph on-the-fly directly from the provided input PAG. This eliminates the need for an external algorithm, such as the one used by $L_{FC}$, to build the call graph. Thus, $L_{DCR}$ is considered complete in the sense that it incorporates call graph construction within its formulation, while $L_{FC}$ does not. The specific method of constructing the PAG for $L_{DCR}$ is irrelevant in this context as long as it is sound. For instance, instead of using CHA, context-insensitive pointer analysis can also be employed to build the initial call graph.

Nevertheless, we have revised our statement to describe our formulation as a specification of callsite-sensitive pointer analysis with built-in on-the-fly callgraph construction. We have made the necessary adjustments throughout the paper by removing the terms "complete specification" and "incomplete". 

Thanks.


```
The authors claimed that one of the contributions is a CFL-reachability formulation of kCFA, but also mentioned that it is not possible to compare directly L_{DCR} with kCFA because L_{DCR} is typically used to implement demand-driven pointer analysis or pre-analysis. Then it looks that the contribution should be “a CFL-reachability formulation for demand-driven analysis / pre-analysis for kCFA”. Indeed, if L_{DCR} cannot entirely replace kCFA, then it doesn’t make sense to call it “a formulation for kCFA”. As a result, I don’t agree with the claim “callsite-sensitive pointer analysis is a special kind of context-sensitive language that can be expressed as the intersection of several CFLs”, since L_{DCR} really doesn’t represent the full power of callsite-sensitive pointer analysis.
```

**Authors:** 
The $L_{DCR}$ formulation presented in this paper is not intended as a replacement for $k$-CFA, but rather as an alternative formulation for expressing callsite-sensitive pointer analysis. $L_{DCR}$ is a CFL-reachability formulation specifically suited for implementing demand-driven analyses, while $k$-CFA is a constraint-inclusion-based formulation primarily used for implementing whole-program analyses.

We have now added a new paragraph in Lines 1018-1026 to highlight the equivalence and disparity between these two analysis approaches (which are well-known by the program analysis community). In addition, we have included a new example in Figure 11. This example is explained in the newly added Lines 1046-1063, offering a detailed illustration of the similarities and differences between demand-driven and whole-program analyses.

```
Combining the above two aspects, I don’t agree with the first contribution listed at the end of the introduction.

Overall, I don’t recommend acceptance of the current version of the paper to TOPLAS, as the actual contributions don’t match what is claimed. However, the work could be promising for more specific scopes. For example, by restricting the scope to pre-analysis, the current work could be regarded as a “precision-preserving pre-analysis”, which could reveal more opportunities for research.
```
**Author:**
This paper makes two contributions. The primary contribution is a CFL-reachability formulation for callsite-sensitive pointer analysis, which incorporates on-the-fly callgraph construction. The secondary contribution is a precision-preserving pre-analysis technique designed to accelerate $k$-CFA while maintaining analysis precision. The second contribution builds upon the first one, utilizing the CFL-reachability formulation.

Please refer to the newly added paragraph (Lines 1018-1026) and the example given in Figure 11, which illustrate the equivalence and disparity between the $L_{DCR}$-based analysis and whole-program analysis.

Thanks.

# Referee: 3
```
Comments to the Author
Overall, my previous comments have been addressed well in this revision.  It is interesting to learn that the recent Eagle work for object sensitivity via CFL-reachability also achieves on-the-fly call graph construction, via what appears to be a similar technique (at a high level).  There is some significant work required to adapt this technique for k-CFA.  However, I think the sentence "We are not aware of any earlier attempt on studying this problem in the literature" (lines 80--81) to be a bit strong, given the Eagle work (and also the Sridharan PhD thesis).  This sentence should be rephrased to succinctly capture what is novel here vs. the object-sensitive work at least; the details can be left to the new Figure 8 and its discussion.
```
**Authors:** 
We have revised the sentence to highlight our contribution in relation to the existing approach of Eagle and Sridharan's formulation.

We have also modified the sentences in the discussion of Figure 8 to emphasize that the CFL-reachability formulation for object-sensitive pointer analysis can utilize an allocation-site-based dispatch approach (Lines 807-808). This is possible because object sensitivity employs receiver objects as context elements.

```
I have the following additional minor comments.

* Line 230: Until -> Unlike
```
**Authors:** Fixed.
```
* Lines 1015-1016, "However, the converse is not true...": This sentence is phrased as if this difference is a well-known fact, and I am not sure that it is.  If a citation could be provided, that would be great.  Otherwise, a small example to illustrate would be helpful.
```
**Authors:** 
We have added a new paragraph in Lines 1018-1026 to highlight the equivalence and disparity between these two analysis approaches (which are well-known by the
program analysis community). In addition, we have included a new example in Figure 11. This example is explained in the newly added Lines 1046-1063, offering a detailed illustration of the similarities and differences between demand-driven and whole-program analyses.


```
* Line 1577: To me it is a bit strange to make reference to the review process here.  I think it's better to write the full paper in a manner consistent with the knowledge that the authors currently have.  You could perhaps state that the previous formulation never appeared in a peer-reviewed publication, if that is what this text is trying to express.
```
**Authors:** Rephrased, Thanks.