Download Link: https://assignmentchef.com/product/solved-racket-homework-3-interpreter-for-the-set-operation-language
<br>



<strong>PART A: </strong><sub> </sub>

<h1>Implementing the SOL Language</h1>




In this part, you are going to implement the Set Operation Language (SOL). We will think of a Set as a sequence of numbers, containing no repetitions. The SOL language will come with three primitive operations: Union, Intersection, and Scalar Multiplication (where each element of the set is multiplied by the same scalar). The language will also support identifiers – and will have ‘with’ expressions, similarly to the WAE language that we have seen in class.




You are requested to fill in all missing parts in the following <a href="https://drive.google.com/open?id=1c9Arah9rW5R9xAfmXo07AZuMyasq4YR3&amp;authuser=omrier%40g.ariel.ac.il&amp;usp=drive_fs">incomplete interpreter</a> (note that each &lt;– fill in –&gt; may stand for more than a single phrase).




<strong> </strong>

<strong>In each of the following parts you are asked to add comments and tests as explained above. </strong>




<strong>Note that to allow submissions for both parts of the assignment as a single file – the names of constructors in part 1 should be slightly different than what we are used to. Specifically, you are asked to augment the names of relevant constructors and functions with the letter S. This would yield, for example, names like IdS, parseS, etc. See more in the basis </strong><a href="https://drive.google.com/open?id=1c9Arah9rW5R9xAfmXo07AZuMyasq4YR3&amp;authuser=omrier%40g.ariel.ac.il&amp;usp=drive_fs">incomplete</a> <a href="https://drive.google.com/open?id=1c9Arah9rW5R9xAfmXo07AZuMyasq4YR3&amp;authuser=omrier%40g.ariel.ac.il&amp;usp=drive_fs">interpreter</a> <strong>provided to you. </strong>




<h2>1. Writing the BNF for the SOL language</h2>




The first step would be to complete the BNF grammar for the SOL language in the partial code provided to you as a skeleton (in the above link to the <a href="https://drive.google.com/open?id=1c9Arah9rW5R9xAfmXo07AZuMyasq4YR3&amp;authuser=omrier%40g.ariel.ac.il&amp;usp=drive_fs">incomplete interpreter</a><a href="https://drive.google.com/open?id=1c9Arah9rW5R9xAfmXo07AZuMyasq4YR3&amp;authuser=omrier%40g.ariel.ac.il&amp;usp=drive_fs">)</a>. In writing your BNF, you can use &lt;num&gt; and &lt;id&gt; the same way that they were used in the WAE language.




<u>The following are valid expressions:</u>

<strong>“{1 3  4 1 4  4 2 3 4 1 2 3}” </strong>

<strong>“{}” </strong>

<strong>“{union {1 2 3} {4 2 3}}” </strong>

<strong>“{intersect {union {1 2 3} {7 7 7}} {4 2 3}}” </strong>

<strong>“{with {S {intersect {1 2 1 3 7 3} {union {1 2 3} {4 2 3}}}}         {union S S}}” </strong>

<strong>“{ scalar-mult 3/2  {with {S {intersect {1 2 1 3 7 3} {union {1 2 3} {4 2 3}}}} </strong>

<strong>                                     {union S S}}}” </strong>

<u>The following are invalid expressions</u>:

<strong>“{1 3  4 1 4  4 2 3 4 1 2 3} {}”     ;; there should appear a single expression </strong>

<strong>“{x y z}”                                      ;; sets cannot contain identifiers </strong>

<strong>“{union {1 2 3} {4 2 3} {}}”          ;; union and intersect are binary operations </strong>

<strong>“{intersect {union {1 2 3} {7 7 7}}}” ;; union and intersect are binary operations </strong>

<strong>“{with S {intersect {1 2 1 3 7 3} {union {1 2 3} {4 2 3}}} </strong>

<strong>       {union S S}}”                         ;;    bad with syntax </strong>

<strong>“{ scalar-mult {3 2} {4 5}}”         ;; first operand should be a nymber </strong>

<strong> </strong>

<strong> </strong>

<strong> </strong>

<strong>  </strong>

<h2>2. Parsing</h2>

Complete the parsing section within the above code. Having understood the syntax, this should be quite straightforward. See the following tests:




<strong>(test (parseS “{1 3  4 1 4  4 2 3 4 1 2 3}”) =&gt; (Set ‘(1 3 4 1 4 4 2 3 4 1 2 3))) </strong>

<strong>(test (parseS “{union {1 2 3} {4 2 3}}”) =&gt; (Union (Set ‘(1 2 3)) (Set ‘(4 2 3)))) </strong>

<strong>(test (parseS “{intersect {1 2 3} {4 2 3}}”) =&gt; (Inter (Set ‘(1 2 3)) (Set ‘(4 2 3)))) </strong>

<strong>(test (parseS “{with S {intersect {1 2 3} {4 2 3}} </strong>

<strong>                 {union S S}}”) </strong>

<strong>      =error&gt; “parse-sexprS: bad `with’ syntax in”) </strong>

<strong>(test (parseS “{}”) =&gt; (Set ‘())) </strong>

<strong> </strong>

<strong> </strong>

<strong> </strong>

<h3>3. Set Operations</h3>

Complete the missing code for the set operations appearing in the incomplete code provided to you. Specifically, complete the code for the following procedures, using the tests that follow below (all already appear in the attached file):




<strong>(: ismember? : Number SET  -&gt; Boolean) </strong>

<strong>(: remove-duplicates : SET  -&gt; SET) </strong>

<strong>(: create-sorted-set : SET -&gt; SET) </strong>

<strong>(: set-union : SET SET -&gt; SET) </strong>

<strong>(: set-intersection : SET SET -&gt; SET) </strong>

<strong>(: set-smult : Number (Listof Number) -&gt; SET) </strong>

<strong> </strong>

<strong>(test (ismember? 1 ‘(3 4 5)) =&gt; #f) </strong>

<strong>(test (ismember? 1 ‘()) =&gt; #f) </strong>

<strong>(test (ismember? 1 ‘(1)) =&gt; #t) </strong>

<strong> </strong>

<strong>(test (remove-duplicates ‘(3 4 5 1 3 4)) =&gt; ‘(5 1 3 4)) </strong>

<strong>(test (remove-duplicates ‘(1)) =&gt; ‘(1)) </strong>

<strong>(test (remove-duplicates ‘()) =&gt; ‘()) </strong>

<strong> </strong>

<strong>(test (create-sorted-set ‘(3 4 5)) =&gt; ‘(3 4 5)) </strong>

<strong>(test (create-sorted-set ‘( 3 2 3 5 6)) =&gt; ‘(2 3 5 6)) </strong>

<strong>(test (create-sorted-set ‘()) =&gt; ‘()) </strong>

<strong> </strong>

<strong>(test (set-union ‘(3 4 5) ‘(3 4 5)) =&gt; ‘(3 4 5)) </strong>

<strong>(test (set-union ‘(3 4 5) ‘()) =&gt; ‘(3 4 5)) </strong>

<strong>(test (set-union ‘(3 4 5) ‘(1)) =&gt; ‘(1 3 4 5)) </strong>

<strong> </strong>

<strong>(test (set-intersection ‘(3 4 5) ‘(3 4 5)) =&gt; ‘(3 4 5)) </strong>

<strong>(test (set-intersection ‘(3 4 5) ‘(3)) =&gt; ‘(3)) </strong>

<strong>(test (set-intersection ‘(3 4 5) ‘(1)) =&gt; ‘()) </strong>

<strong> </strong>

<strong>(test (set-smult 3 ‘(3 4 5)) =&gt; ‘(9 12 15)) </strong>

<strong>(test (set-smult 2 ‘()) =&gt; ‘()) </strong>

<strong>(test (set-smult 0 ‘(3 4 5)) =&gt; ‘(0 0 0)) </strong>

<strong> </strong>

<h3>4. Substitutions</h3>

Using the following formal specifications (you can also use our WAE interpreter as reference), write the subst procedure

<strong>(: substS : SOL Symbol SOL -&gt; SOL) </strong>

<strong>  ;; substitutes the second argument with the third argument in the </strong>

<strong>  ;; first argument, as per the rules of substitution; the resulting   ;; expression contains no free instances of the second argument </strong>

<strong>  …. </strong>




<table width="516">

 <tbody>

  <tr>

   <td width="516">  #| Formal specs for `subst’:Formal specs for `subst’:(`Set’ is a &lt;NumList&gt;, E, E1, E2 are &lt;SOL&gt;s, `x’ is some&lt;id&gt;, `y’ is a *different* &lt;id&gt;)Set[v/x]              = Set{smult n E}[v/x]      = {smult n E[v/x]}{inter E1 E2}[v/x]    = {inter E1[v/x] E2[v/x]}       {union E1 E2}[v/x]    = {union E1[v/x] E2[v/x]}       y[v/x]                = y       x[v/x]                = v{with {y E1} E2}[v/x] = {with {y E1[v/x]} E2[v/x]}{with {x E1} E2}[v/x] = {with {x E1[v/x]} E2}   |#</td>

  </tr>

 </tbody>

</table>







<h3>5. Evaluation</h3>

Using the following formal specifications (you can also use our WAE interpreter as reference), and using the set operations you implemented above – write the eval procedure:

<strong>(: eval : SOL -&gt; SET) </strong>

<strong>;; evaluates SOL expressions by reducing them to set values </strong>

<strong> </strong>

#| Formal specs for `eval’:

Evaluation rules:

eval({ N1 N2 … Nl }) = (sort (create-set (N1 N2 … Nl)))         where create-set removes all duplications from         the sequence (list) and sort is a sorting procedure.

eval({scalar-mult K E}) =  (K*N1 K*N2 … K*Nl)     eval({intersect E1 E2})= (sort (create-set (set-intersection                                                     (eval(E1),eval(E2))))     eval({union E1 E2}) = (sort (create-set (eval(E1),

eval(E2))))

eval({with {x E1} E2}) = eval(E2[eval(E1)/x])










<h3>6. Interface</h3>

The run procedure wrapping it all up is provided to you. Use it to write tests for your code.
















<strong>PART B: </strong><sub> </sub>

<h1>Detecting free instances in a WAE program</h1>

A code that contains free instances of an identifier is a bad code, and should not be evaluated into anything but an error message. In this section, we go back to the realm of the WAE language to consider the task of identifying free instances in a program.

<h1>Identifying free instances before evaluation</h1>

To check whether or not there are free instances of an identifier (any identifier) – it is enough to perform a syntactic analysis (that is, no need to evaluate the code).

In our WAE interpreter we indeed issue an error message in such a case, however, we do so during the evaluation process. Write a function   <strong>(: freeInstanceList : WAE -&gt; (Listof Symbol))</strong>

that consumes an abstract syntax tree (WAE) and returns null if there are no free instance, and a list of all the free instances otherwise – each appearing exactly once in the list (i.e., multiple free occurrences of the same identifier that are free should all be represented by a single occurrence of it in the list). The order in which symbols appear within the list is not important.

Here are some tests that should assist you:

(test (freeInstanceList (parse “w”)) =&gt; ‘(w))

(test (freeInstanceList (parse “{with {xxx 2} {with {yyy 3} {+ {- xx y} z}}}”)) =&gt; ‘(xx y z))

(test (freeInstanceList (With ‘x (Num 2) (Add (Id ‘x) (Num 3)))) =&gt; ‘())

(test (freeInstanceList (parse “{+ z {+ x z}}”)) =&gt; ‘(z x))

<strong> </strong>

<u>HINT:</u> use the current structure of eval (and subst) in the WAE interpreter we have seen in class to do so. Your resulting program should never evaluate the code (WAE) it takes as input. In fact, the function eval itself should not appear in your code.


