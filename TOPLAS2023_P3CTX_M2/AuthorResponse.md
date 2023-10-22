# Author Response

We thank all the reviewers for their invaluable input and constructive comments.

We have revised our paper carefully according to the suggestions from the Associate Editor and the three reviewers. Below, we provide our response to the list of major changes requested by the Associate Editor and then the three individual reviewers.

We thank the reviewers for helping us improve both the presentation and the quality of this paper.


```
Associate Editor

Comments for Author:
Your manuscript contains interesting insights in CFL-reachability analysis, but the reviewers point to multiple major issues that need to be addressed before it can be accepted for publication. We would like you to perform the following high-level revisions (please see the reviews for details):
```

We have significantly revised the paper. In particular, Figures 8, 9, and 13 -- 16 are all new. Table 3 has been extended to include the new experimental results.

```
1) Clarify or revise the claims about precision to address the concerns about how the CFL-reachability formulation relates to k-CFA. This includes clarifying the conceptual novelty of the contributions.
```
**Authors:** We have added a new paragraph just before Section 3.3 to relate the precision of $L_{DCR}$ to that of $k$CFA. Please note that this is exactly also how the precision of  Sridharan's $L_{FC} [52] is related to that of $k$CFA, a known fact in the pointer analysis community. Therefore, how both are related doesn't affect the conceptual novelty of our contributions. Nevertheless, we have also elaborated on our two contributions given in Section 1 (lines 104-106 and lines 109-110) to both clarify the conceptual novelty of $L_{DCR}$ and
reflect the new experimental results added in the revised paper.


```
2) Expand the discussion on related work.
```

**Authors:** In Section 5, we have now highlighted the main differences between our work and an earlier attempt made in Sridharan’s PhD thesis for addressing the same problem [51], reviewed a few recent papers focusing on the complexity of interleaved Dyck-CFL reachability, and finally, discussed the main differences between this work and our recent work [33, 35] on providing a CFL-reachability-based formulation for supporting object-sensitivity.

```
3) Extend the experimental evaluation, comparing your approach to state-of-the-art alternatives to demonstrate that it has a better precision/performance balance than existing CFL-style and k-CFA techniques.
```

**Authors:** In Section 4, we have extended our evaluation substantially by comparing P3Ctx (our pre-analysis based on $L_{DCR}$) with Selectx [34] (a pre-analysis based on Sridharan's $L_{FC}$ [52]) and Zipper [29] (a pre-analysis based on pattern-matching with respect to $k$CFA). Our experimental results demonstrate clearly why P3Ctx, and consequently, $L_{DCR}$, advance the state-of-the-art, with P3Ctx providing better efficiency-precision trade-offs in a number of potential application scenarios as highlighted in lines 1505-1515.

In the original submission, we have already compared P-$k$CFA with $k$CFA in Table 3, demonstrating that P-$k$CFA is faster than $k$CFA while always preserving its precision.

It is not possible to compare directly a CFL-reachability-based formulation such as $L_{DCR}$ or Sridharan's $L_{FC}$ [52]) with $k$CFA, as the former is typically used to implement demand-driven pointer analysis and the latter for whole-program pointer analysis. We are not aware of any such a comparison in the literature.


```
4) Fix the presentation issues and answer the questions in the reviews.
```

**Authors:** We have done so throughout the paper and will address the raised questions below.

# Review 1

```
Comments to the Author
The paper is concerned with the problem of context-sensitive pointer analysis for Java, formulated as a CFL-reachability problem. In high-level, pointer analysis tries to approximate the set of heap objects that pointers may point-to at runtime. Naturally, such approximations are conservative, but try to be precise. One approach in this direction is k-CFA, a form of context sensitive analysis in which the points-to information of a pointer is parameterized (and thus refined) by the call stack. One other popular approach is to formulate this problem as a Context-sensitive-language (CFL) reachability on a graph program model known as the Pointer Assignment Graph (PAG).

Although the CFL-reachability formulation is more efficient than k-CFA (i.e., takes less time to solve), it is also less precise. Although there are several points of imprecision, the paper tackles imprecision that comes from the call graph. CFL-reachability usually requires the call graph be established by a separate analysis in a preprocessing step. As a precise call-graph construction itself requires points-to information, this preprocessing step usually performs a more conservative analysis, usually based on class hierarchies. As a result, the underlying graph can be quite coarse, polluting CFL reachability with false positives. In particular, one major challenge comes from the presence of virtual calls. Without precise points-to information, a virtual call is represented by all the concrete calls that it can dispatch in runtime, yielding false flow-to CFL paths in the PAG.

The paper addresses this challenge igcfxderw234erwtdfsgcxvbdfn two conceptual steps. First, it proposes a new PAG model, that explicitly captures the classes of heap objects with edges that have appropriate labels of the form "new[T]", with T being the class. Method call edges have a "dispatch[T]" label, with T being the type of the concrete method being invoked. On the CFL-reachability side, matching "new[T]" with "dispatch[T]" allows to focus on flow-to-paths between objects and methods of the same type. Further labels on field accesses allow to more precisely match the receiver object of a call to its "this" references inside the invoked method. In the end, the analysis question resorts to an interleaved CFL Reachability question with respect to three languages (i.e., their intersection), L_{C}, L_{D}, and L_{R}, where L_{C} captures call-context sensitivity, L_{D} captures sensitivity to fields but also dynamic method invocations (a contribution of this work), and L_{R} for increasing the precision of parameter passing on virtual callsites (another contribution of this work). As the reachability problem is undecidable, the paper proposes some further (k-limiting) regularizations, to end up with a plain CFL-reachability problem.

On the experimental side, the authors use the new approach to accelerate k-CFA reachability,
by performing a precision-preserving pre-analysis for  approximating the contexts. Concretely, this was applied for k=1 and k=2, with varying speedups between 1.2X and 5X.


The paper is based on some nice insights that manage to increase the precision of pointer analyses based on CFL-reachability formulations. However, I have reservations for recommending for acceptance in TOPLAS. In the bigger scheme of things, the main selling point is the CFL formulation of dynamic method dispatching. But this is done by introducing a Turing-complete formulation (interleaved CFL reachability). So at the fundamental level, the contribution is hardly surprising (as a remark, interleaved CFL reachability has also been used in the past, but for handling context and field sensitivity. The formulation proposed here introduces an orthogonal component of Turing-completeness). Naturally, to deal with Turing completeness the method is reduced to a less expressive formalism, by introducing regularizations of some of the initial CFLs. However, these regularizations are not evaluated in neither theory nor practice. This is even more so as many follow ad-hoc-looking decisions. Some are borrowed from earlier work, but the question of what is fundamentally new and scientifically novel remains unclear to me.

One would expect that a thorough experimental evaluation would instead support this new approach, but the current experimental section does not achieve this. The superiority of the new pointer-analysis formulation compared to other efficient, CFL-style methods is not tested. The experiments are only concerned with using the new method to perform a pre-analysis in k-CFA-based pointer analysis. Naturally, a Turing complete-formulation as a pre-analysis of an otherwise decidable analysis is plainly odd. Of course, it is only the simplified (regularized, non-Turing-complete) approach that is used, but that is not precision-preserving, as (falsely) advertized. The benefits then must be shown experimentally, but again, there is no comparison with other, non-precision-preserving pre-analyses. It is also unclear to me what one benefits from by guaranteeing a precision-preserving pre-analysis, since a precise analysis follows anyway.

Finally, some parts of the paper lack the necessary rigor for a PL journal (see below for some specific comments). In my opinion, revising these and further demonstrating the practical benefit of the new approach via more thorough experiments could make for a paper of interest to the pointer-analysis community.
```

**Authors:** This paper introduces a new CFL-reachability formulation for $k$CFA with call-graph construction built-in. Despite its Turing completeness, this formulation represents the first solution ever proposed, demonstrating theoretically for the first time that call-site-sensitive pointer analysis is a special context-sensitive language that can be expressed as the intersection of three CFLs. We have now highlighted this in lines 85-87 and 104-106. We have also demonstrated how it can be leveraged to develop a pre-analysis for speeding up $k$CFA while preserving its precision for the first time. Note that all the existing pre-analyses for accelerating $k$CFA will cause $k$CFA to lose precision.

There is a misunderstanding about the term "precision-preserving" used in the paper. We apologize if we haven't explained this clearly in the original submission. Regularizing $L_{DCR}$ is certainly not precision-preserving in the sense that the regularized one represents an over-approximation of $L_{DCR}$. In our paper, P3Ctx (a pre-analysis developed based on this regularized $L_{DCR}$) is said to be precision-preserving since when we use P3Ctx to guide $k$CFA to support selective context-sensitivity (i.e., by applying context-sensitivity to only a subset of variables and objects), the modified $k$CFA this way (referred to as P-$k$CFA) achieves exactly the same precision as $k$CFA. Hence, a pre-analysis such as P3Ctx is commonly referred to as being precision-preserving in the pointer analysis literature.

In Section 4.2, we have now included a thorough experimental evaluation comparing P3Ctx with two state-of-the-art non-precision-preserving pre-analyses, Selectx [34] (a pre-analysis based on $L_{FC}$ [52]) and Zipper [29] (a pre-analysis based on pattern-matching with respect to $k$CFA). Our evaluation demonstrates clearly why P3Ctx, and consequently, $L_{DCR}$, advance the state of the art, with P3Ctx providing better efficiency-precision trade-offs in a number of potential application scenarios as highlighted in lines 1505-1515. We have also added two real-world examples in Figures 14 and 15 to explain why Selectx (based on $L_{FC}$) cannot be precision-preserving. Note that in the original submission, we have already compared P-$k$CFA with $k$CFA in Table 3, demonstrating that P-$k$CFA is faster than $k$CFA while always preserving its precision. It is not possible to compare directly a CFL-reachability-based formulation such as $L_{DCR}$ or Sridharan's $L_{FC}$ [52] with $k$CFA, as the former is typically used to implement demand-driven pointer analyses (or pre-analyses such as P3Ctx) and the latter for whole-program pointer analyses. We are not aware of any such direct comparison in the literature.

Finally, regarding "It is also unclear to me what one benefits from by guaranteeing a precision-preserving pre-analysis, since a precise analysis follows anyway", context-sensitive pointer analysis algorithms are still inefficient or unscalable for large codebases. For major IT companies such as Google (using its Tricoder) and Huawei （via its CodeArts), over 100 million lines of code are statically analyzed daily. Often, each program is usually allocated with a small time budget, say, 1 hour (or even less). Developing faster yet precise pointer analysis algorithms P-$k$CFA will enable $k$CFA to analyze more applications to completion than ever before while still providing the same precision as expected by these applications.


```
Some further comments in the text
--------------------------------------

l376. The phrasing of this sentence is odd. If L_{FC} stands for the same language as previously, why is the path in Eq(4) not an L_{FC} path?
```

**Authors:** For $L_{FC}$, the inter-procedural assign edges (i.e., call edges) related to a call do not directly appear in the PAG but are added on the fly by a separate call graph construction algorithm (as explained in the original submission (Section 2)). In Eq(3), we reach variable d under context [c1] and find that variable x points to A1 under context [c1]. Thus, $d \xrightarrow{assign} p$ is a call edge to be added to the PAG. In Eq(4), however, we reach d under context [c2] and find that variable x points to B1 under context [c2]. Thus,  $d \xrightarrow{assign} p$ cannot be a call edge (as the target should be B:foo(), not A:foo()). Therefore, we know that the path in Eq(3) is an $L_{FC}$ path but the path in Eq(4) is not. 

We have now rephrased this sentence. Thanks.

```
l526 (and throughout). Maybe "for addressing challenge X" is better phrasing than "by addressing challenge X".
```
**Authors:** Changed. 

```
p12, Fig 6. In the [C-PARAM] rule, define what p_i^M are.
```

**Authors:** $p_i^M$ is defined in line 584. We have now duplicated this definition in [C-PARAM].

```
p13, Definition 1. The text in the definition is way too vague and amount to a formal definition. Statements like "be a CFL-reachability formulation", "how they handle parameter passing", "its own built-in call graph" lack the rigor required for the definition. This is important, as soundness and precision statements (the main formal results of the paper) rest on this definition.
```

**Authors:** Rephased. Thanks.


```
p14, Lemma 1. The lemma lacks some formalism. In particular, "can be dispatched on the received object" is undefined. What does "can be dispatched" mean? According to what semantics?
```
**Authors:** As mentioned in line 52, we present our formulation in Java. Thus, for a
virtual callsite, dynamic method dispatch is performed based on the dynamic type of its
receiver object. 

We have now mentioned Java explicitly in lemma 1. Thanks.

```
p15, Lemma 2. Similarly to the previous comment, "sound in handling parameter passing" is too vague a statement to appear in a formal lemma.
```

**Authors:** The notion of soundness is given in Definition 1, which has now been rephrased.
Basically, $L_{DC}$ is sound in handling parameter passing at a virtual callsite if it can perform parameter passing for a superset of the set of target methods dispatched at the callsite, which is now stated informally just before this lemma is stated.

```
l732. Is the calligraphic notation on L_R intentional? This is not important, but I did wonder whether it signifies a difference compared to, e.g., L_{DC}.
```
**Authors:** This was a mistake and is now fixed. Thanks.

```
p19, Section 3.3. The undecidability of L_{FC}, L_{DCR}, etc, for graphs that have reverse edges was shown in [A]. The paper [38] only concerns graphs that do not have reverse edges, and does not apply in this setting.
```

**Authors:** The PAGs of $L_{FC}$ and $L_{DCR}$ indeed have reverse edges but they are not bidirected graphs. According to the definition given in [A]: `A Dyck graph is bidirected if every edge is present in both directions, and the two mirrored edges have complementary labels. That is, a --> b opens a parenthesis iff b --> a closes the same parenthesis.` However, the inverse edges in our PAG do not satisfy this condition. For example, in our PAG, the inverse of a store edge $a \xrightarrow{store[f]} b$ is $b \xrightarrow{\overline{store[f]}} a`$, but `store[f]` can only be matched by `load[f]`, and similarly, $\overline{load[f]}$ can only be matched by $\overline{store[f]}$. Thus, our PAG is not bidirected and [45] ([38] in the original submission) can therefore be applied in Section 3.3.

We have extended Section 5 by discussing more recent works on the complexity of interleaved Dyck-CFL reachability on both general and bidirected graphs. We have also discussed the differences between our PAG and the bidirected graph. Thanks for providing Reference [A].

```
p21, Lemma 3. Technically, the proof does not correspond to the lemma, as the inclusions in the proof are non-strict, but the lemma states a strict inclusion.
```

**Authors:** The inclusion in the lemma should be non-strict, too. Thanks for pointing this out.

```
[A] Adam Husted Kjelstrøm and Andreas Pavlogiannis. 2022. The decidability and complexity of interleaved bidirected Dyck reachability. Proc. ACM Program. Lang. 6, POPL, Article 12 (January 2022), 26 pages. https://doi.org/10.1145/3498673
```

# Review 2

```
Comments to the Author
# Paper summary
This paper presents a CFL-reachability formulation of k-CFA for Java with built-in on-the-fly call graph construction. The formulation consists of the intersection of three CFLs (which is referred to as L_DCR) with a new version of PAG (program assignment graph). In practice, two of the three CFLs are approximated by regular languages. The new method is applied to a pre-analysis of selective context-sensitivity for k-CFA. This pre-analysis is precision-preserving, and experimental results on a set of Java benchmarks show that it can accelerate subsequent 1-CFA and 2-CFA without precision loss.

# Assessment
First, the paper claims that L_DCR formulates k-CFA completely in terms of CFL-reachability, but I don’t fully agree with this. As the paper mentioned in Section 3.1, when building the PAG, an over-approximation of the set of target methods invoked at each callsite is still needed, which is realized by CHA (class hierarchy analysis). So a separate (perhaps imprecise) call graph construction is still needed. Although L_DCR will perform call graph construction again and filter out some spurious call targets later, the first step is not avoided, and thus, I don’t think L_DCR is a complete formulation of k-CFA solely in CFL-reachability.
```

**Authors:** We believe our CFL-reachability formulation provides a complete specification for call graph construction for four reasons. First, every CFL-reachability formulation must be defined in terms of a given (fixed) PAG (Section 2.1.2), implying that both are mutually independent. Second, pre-building the PAG for a program by using CHA yields a sound (i.e., over-approximated) call graph so that the spurious ones can be filtered out by our CFL-reachability formulation. Third, CHA relies only on the type information for each receiver variable at a virtual callsite. Finally, the time complexity for building such an over-approximated call graph initially by using CHA is linear (in terms of the program size). 

```
Second, the paper evaluates the new method as a pre-analysis for k-CFA, but the new method is claimed to be a formulation of k-CFA as well. The paper didn’t perform a direct comparison between the new method and existing k-CFA algorithms like the Andersen-style algorithm shown in Figure 1.
```
**Authors:** In the paper, we have regularized $L_{DCR}$ and designed a pre-analysis, P3Ctx, for accelerating $k$CFA with selective context-sensitivity while preserving its precision. The version of $k$CFA for performing selective context-sensitivity prescribed by P3Ctx is referred to as P-$k$CFA in our paper. In Table 3, we have already compared P-$k$CFA with $k$CFA, demonstrating that P-$k$CFA is faster than $k$CFA without losing any precision.

In the revised version, we have also compared P-$k$CFA with two state-of-the-art pointer analyses, S-$k$CFA (the version of $k$CFA for supporting selective context-sensitivity by SELECTX) and Z-$k$CFA (the version of $k$CFA for supporting selective context-sensitivity by ZIPPER). We have also added two new examples (in Figures 14 and 15) to explain why existing pre-analyses such as S-$k$CFA can lose precision in real-world applications.

It is not possible to compare directly a CFL-reachability-based formulation such as $L_{DCR}$ or Sridharan's $L_{FC}$ [52] with $k$CFA, as the former is typically used to implement demand-driven pointer analyses (as described in Section 1) or pre-analyses (such as P3Ctx as demonstrated in this paper), while the latter is used to implement whole-program pointer analyses. We are not aware of any such direct comparison in the literature.

Thanks.

```
Finally, the writing of the paper could be improved (see the following detailed comments).

# Detailed comments
(Line numbers refer to the red numbers on the left of every page.)
Lines 9-11: I don’t understand this limitation. In particular, what is “meta-analysis”, and which k-CFA’s precision is reduced (I assume you are referring to the pre-analysis case where a subsequent k-CFA will be performed)?
```
**Authors:** In the original submission, a meta-analysis (e.g., a pre-analysis) is referred to as an analysis that is developed to analyze another analysis (e.g., a main pointer analysis). We have removed this to avoid any confusion in the revised paper.
 

Since $L_{FC}$ [52] represents an incomplete specification of $k$CFA (as explained in the original submission), any $L_{FC}$-based pre-analysis, such as Selectx [34], that is designed to accelerate $k$CFA with selective context-sensitivity, will end up causing $k$CFA to lose precision, as demonstrated by our evaluation in this paper.


```
Line 139: What is the exact mechanism of “dispatch” (it must be some kind of approximation)?
```
**Authors:** As described in our paper, we present our formalization for Java. Therefore, Java's standard single-dispatch semantics is used. Given a virtual call `a.foo(...)`, once the runtime type of `a` is known, the target method of this virtual call will be uniquely determined. Thus, there is no approximation happening here.  We have rephrased these few sentences in the revised paper. Thanks.

```
Figure 1: What is “htx”?
```
**Authors:** In Figure 1, `htx` is the (heap) context of `O`, as mentioned in lines 179-181 and illustrated in Table 2. We have now duplicated this information in the caption of Figure 1. 

```
Line 613: How is T defined (the exact set must be undecidable so T should be some approximations as well)?
```
**Authors:** For any practical implementation (under $k$-limiting), T is computed exactly by a separate on-the-fly callgraph construction algorithm. We have now made this clear in Definition 1. Thanks.

```
Line 915: It is wrong to say, “make the L_DCR-reachability problem computable in polynomial time,” and indeed, this is only an approximation, not the exact “L_DCR-reachability problem.”
```
**Authors:** Our statement is correct since the phrase 'apply $k$-limiting to $L_R$ and $L_C$' already means that the $L_{DCR}$-reachability problem will be approximated. 

```
Line 966 mentions that the actual formulation is L_DC instead of L_DCR in the evaluation, and lines 1021-1023 say that after regularization, the only difference between P3CTX and SELECTX is the underlying PAG representation. This should be mentioned earlier in the introduction.
The motivating example (Section 2) is too long (occupying about 8 pages).
# Questions
The actual formulation used in the pre-analysis is L_DC instead of L_DCR (line 966), so why don’t you use L_DCR?
```
**Authors:** When designing P3Ctx, we must make it lightweight so that we can efficiently identify precision-critical variables/objects without losing the performance benefits obtained from the subsequent main pointer analysis. Since P3Ctx is used to make context-sensitivity selections for $k$CFA, we must keep $L_C$ unchanged and choose to regularize both $L_{D}$ and $L_R$. In the revised paper (lines 1088-1096), we have now explained why regularizing $L_R$ makes it directly ignorable. Then we proceed to regularize  $L_D$ as described originally in the paper. Finally, we obtain an over-approximated language for developing our pre-analysis P3Ctx.

Thanks for asking us to clarify this, as this part of the paper should become clear now.


```
When building the PAG for L_DCR, if we just over-approximate each callsite’s target methods as all possible methods in the program, then the CHA is not needed, and perhaps L_DCR can be regarded as a “complete formulation in CFL-reachability.” In this case, will the performance be acceptable?
```
**Authors:** As explained earlier, $L_{DCR}$ represents a complete formulation of CFL-reachability since applying CHA to pre-build an over-approximated call graph for a program is linear (in terms of the program size) by leveraging only the type information available in the program. Yes, if one just over-approximates each callsite’s target methods as all possible methods in the program, CHA is definitely not needed, but more time will certainly be needed to filter out more spurious target methods. In practice, to support context-sensitive pointer analysis algorithms and many existing pre-analyses for supporting selective context-sensitivity [38,39,34,29] (Section 5), the initial over-approximated call graph of a program is usually pre-constructed by applying a context-insensitive Andersen-style pointer analysis, which can be more expensive than CHA. However, the time overhead incurred in obtaining such an initial call graph that is as precise as possible will usually be significantly more than offset by the performance benefits obtained by the subsequent main pointer analysis performed.



# Review 3

```
Comments to the Author
Overall, I found this to be a good paper that I think will be of interest to the program analysis community.  The core issue of on-the-fly call graph construction has not been explicitly discussed in CFL-reachability formulations of points-to-analysis in the literature, though it fits fairly naturally into constraint-based specifications (as shown in Figure 1).  This paper gives a relatively clean way to bridge that gap, and in turn, provides insights into the benefits and limitations of using CFL-reachability for this problem.  I do have a number of comments and concerns, which I outline below.  But I believe they should be addressable in a revision.

Prior work
----------

While it is true that a formulation of on-the-fly call graph construction in CFL-reachability does not appear in the peer-reviewed literature, there is a prior formulation in a technical report, Sridharan's Ph.D. thesis:

https://www2.eecs.berkeley.edu/Pubs/TechRpts/2007/EECS-2007-125.html

See Section 3.4 of the above.  The thesis was not peer-reviewed, so it does not impact the publishability of the current paper.  Still, I believe this paper should include a comparison with the above, given there seem to be many similarities between the formulations.  That is not to say they are identical: at a first glance, it seems the current formulation has some advantages over the previous one.  For one, fewer new types of edge labels are introduced, since the current formulation re-uses load and stores edges to model parameter passing, a clear improvement.  Also, it seems to be the current formulation is more precise: I don't believe the example of Figure 8 would be handled precisely by the previous formulation.  In any case, it would be great if the paper gives a more detailed comparison.
```
**Authors:** Thanks for sharing this existing formulation with us. Appreciated! We have now included a detailed comparison between our formulation $L_DCR$ and this existing formulation in Section 5.1. As acknowledged by this reviewer, this existing formulation is less precise than $L_DCR$ due to the lack of L_R as illustrated in Figures 9 and 10. 

```
Second, the formulation in this paper has similarities with the formulation of object-sensitive analysis as CFL-reachability in the authors' TOSEM 2021 paper on Eagle.  In particular, the Eagle formulation also models parameter passing and returns using a form of loads and stores (see Fig. 7 in that paper).  And, if I understand it correctly, CFL paths for parameter passing / returns in the TOSEM formulation will include a sub-path from the receiver variable at a virtual call site to the abstract object being passed as the receiver, an essential part of handling dynamic dispatch.  That paper also claims that "spurious PAG nodes and edges introduced will obviously not be traversed by a more precise pointer analysis," where "spurious" edges are those introduced by a less precise pre-computed call graph.  It is unclear to me whether the TOSEM'21 formulation achieves an on-the-fly call graph.  The paper should give a more detailed comparison here, and if the TOSEM'21 work does not achieve an on-the-fly call graph, it should give an example showing why.
```
**Authors:** We have now added a new figure (Figure 8) to illustrate three different ways of performing dynamic method dispatch for a virtual call site. We have also explained why $L_DCR$ must perform method dispatch directly at a callsite in order to support callsite-based context-sensitivity (Figure 8b). In contrast, our CFL-reachability formulation for supporting object-sensitive pointer analysis [33, 35] can perform method dispatch directly at an object allocation site (Figure 8c), and consequently, on-the-fly call graph construction, due to the nature of object-sensitivity, as discussed now in more detail in Section 5.1. It is important to emphasize that it is challenging to develop $L_{DCR}$ for supporting callsite-based context-sensitivity, since parameter passing for a virtual callsite is not directly related to its receiver object.


```
Details of formulation
----------------------

I like the use of above and below labels on edges to more cleanly separate concerns of calling contexts from those of fields and dynamic dispatch.  Standard CFL-reachability formulations only use a single label per edge.  But I believe the above and below labels are just a syntactic sugar to keep the grammars cleaner, and they can be "translated away" so that each edge only has a single label, at a cost of grammar complexity.  The paper should clarify.
```
**Authors:** We have now clarified this in lines 230-234, 239-243, 253-260, 270-275, 632-633, 687-689, and 890-893. A simple transformation will make both representations equivalent. Thanks for asking us to add this clarification in the revised paper.


```
The paper should further clarify the exact role of the L_{R} language.  The example of Figure 8 makes it seem that L_{R} is primarily about not losing context information when determining a target of a virtual call.  But I believe that L_{R} is required for basic matching of call sites even within a single method.  Consider the following example:

class A {
   void m(Object p) { ... }
   void n(Object q) { ... }
   main() {
       A a = new A(); // A1
       Object o1 = new Object(); // O1
       Object o2 = new Object(); // O2
       a.m(o1);
       a.n(o2);
   }
}

I believe there are $L_{DC}$ paths for this example from O1 to q and from O2 to p, since o1 and o2 are both passed in parameter position 1, which is clearly imprecise.  Is this correct?  If so, and if it is L_{R} that removes this imprecision, this should be clarified.  (Example 9 also shows that L_{DC} paths do not consider virtual call site matching.)
```
**Authors:** Yes, you are correct. In $L_{DC}$,  O1 can flow to q and O2 can flow to p spuriously, and $L_R$ will remove this imprecision.

$L_{R}$ plays two roles, by ensuring that (1) a method dispatch is always performed at a correct call site, and (2) a method dispatch is always performed under a correct context. The example in Figure 10 (i.e., Figure 8 in the initial submission) serves to illustrate the second role explicitly but the first role implicitly. We have now included your example above in Figure 9 in our revised paper to highlight the first role of $L_R$. Thanks.

```
The L_{R} grammar would be easier to understand with more descriptive names for the non-terminals.  Maybe $W$ could be something like $UB$ for unbalanced call sites, and $R$ could be like $M$ for matched call sites?  Or full English words could be used like flowsTo and alias in the L_{D} grammar.  Some good naming here would help a lot in terms of understandability.
```
**Authors:** Thanks for your suggestion. We have now used some more descriptive names.

```
I am curious about the L_{CR} language, which is not explicitly discussed in the paper.  Is L_{CR} context-free?  Or is computing L_{CR} reachability on its own undecidable due to interleaved parenthesis languages?  It would be good to know if this formulation is introducing yet another source of undecidability.
``` 
**Authors:** We have now added some discussion on this in Section 3.3


```
Proofs and precision
--------------------

I personally did not find the proofs in the paper to be very useful.  They are too informal to give strong confidence in the results, and it is not clear to me the theorems are really the most interesting ones to think about.  What _would_ be interesting would be to formally relate the CFL-reachability formulation to the constraint-based formulation given in Figure 1.  Here, I believe the CFL-reachability formulation is actually _less_ precise than the Figure 1 formulation, due to issues with unreachable code.  Consider the following example:

class A {
 void m(Object p) { … }
}
class Main {
 void main() {
   A a1 = new A(); // A1
   Object o1 = new Object(); // O1
   a1.m(o1);
 }
}
class B {
 void n() {
   A a2 = new A(); // A2
   Object o2 = new Object(); // O2
   a2.m(o2);
 }
}

In this example, B.n() is unreachable since it is not called from Main.main().  However, B.n() would be included in a precomputed call graph computed with class-hierarchy analysis, and hence would be present in the PAG as constructed for this paper.  Hence, I believe the PAG will have an L_{DCR} path from O2 to p.  In contrast, the Fig. 1 analysis will _not_ compute this result, since it only analyzes code determined reachable by the analysis itself (via the $ctx \in MethodCtx(m)$ condition in each rule); so it will never process statements in B.n().  This remains a potential issue anytime the call graph used for PAG construction is computed with less precision than the analysis itself.

The paper should consider cases like the above and explain exactly how the precision of the CFL-based analysis relates to that of Fig. 1.
```
**Authors:**  We are certainly aware of this difference. We have added a new paragraph just before Section 3.3 to highlight the relationship between the two. Please note that the same relation between the precision of $L_{FC}$ and the precision of $k$CFA also exists.

```
Experiments
-----------

I am unsure about some decisions made in the experiments.  First, in Section 4.1.2, why is L_{DC} used as the starting point for regularization rather than L_{DCR}?  As noted above, L_{DC} could have unexpected imprecision since it does not even do matching of call sites.
```

**Authors:** When designing P3Ctx, we must make it lightweight so that we can efficiently identify precision-critical variables/objects without losing the performance benefits obtained from the subsequent main pointer analysis. Since P3Ctx is used to make context-sensitivity selections, we must keep $L_C$ unchanged and choose to regularize both $L_{D}$ and $L_R$. In the revised paper (lines 1087-1096), we have now explained why regularizing $L_R$ makes it directly ignorable. Then we can proceed to regularize  $L_D$ as described originally in the paper. Finally, we obtain an over-approximated language for designing P3Ctx.

```
Second, why do the experiments not contain any comparison with SelectX?  It seems that if the paper wants to show the value of this new formulation, the experiments would show some improvement of pre-analysis with P3Ctx as compared to SelectX.  This is particularly important given the potential imprecision from starting with L_{DC} rather than L_{DCR}.  The experimental comparison with SelectX should be added.
```

**Authors:** In Section 4.2, we have now included a thorough experimental evaluation comparing P3Ctx with two state-of-the-art non-precision-preserving pre-analyses, Selectx [34] (a pre-analysis based on $L_{FC}$ [52]) and Zipper [29] (a pre-analysis based on pattern-matching with respect to $k$CFA). Our evaluation demonstrates clearly why P3Ctx, and consequently, $L_{DCR}$, advance the state of the art, with P3Ctx providing better efficiency-precision trade-offs in a number of potential application scenarios as highlighted in lines 1505-1515. We have also added two real-world examples in Figures 14 and 15 to explain why Selectx (based on $L_{FC}$) cannot be precision-preserving.
