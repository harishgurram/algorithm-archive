<script>
MathJax.Hub.Queue(["Typeset",MathJax.Hub]);
</script>
$$ 
\newcommand{\d}{\mathrm{d}}
\newcommand{\bff}{\boldsymbol{f}}
\newcommand{\bfg}{\boldsymbol{g}}
\newcommand{\bfp}{\boldsymbol{p}}
\newcommand{\bfq}{\boldsymbol{q}}
\newcommand{\bfx}{\boldsymbol{x}}
\newcommand{\bfu}{\boldsymbol{u}}
\newcommand{\bfv}{\boldsymbol{v}}
\newcommand{\bfA}{\boldsymbol{A}}
\newcommand{\bfB}{\boldsymbol{B}}
\newcommand{\bfC}{\boldsymbol{C}}
\newcommand{\bfM}{\boldsymbol{M}}
\newcommand{\bfJ}{\boldsymbol{J}}
\newcommand{\bfR}{\boldsymbol{R}}
\newcommand{\bfT}{\boldsymbol{T}}
\newcommand{\bfomega}{\boldsymbol{\omega}}
\newcommand{\bftau}{\boldsymbol{\tau}}
$$

# Tree Traversal 

Trees are naturally recursive data structures, and because of this, we cannot access their elements like we might access the elements of a vector or array. Instead, we need to use more interesting methods to work through each element. This is often called *Tree Traversal*, and there are many different ways to do this. For now, we will restrict the discussion to two common and related methods of tree traversal: *Depth-First* and *Breadth-First Search*. Note that trees vary greatly in shape and size depending on how they are used; however, they are composed primarily of nodes that house other, children nodes, like so:

{% method %}
{% sample lang="jl" %}
[import:4-8, lang:"julia"](code/julia/Tree.jl)
{% sample lang="cpp" %}
[import:1-4, lang:"c_cpp"](code/c++/tree.cpp)
{% sample lang="cs" %}
[import:11-15, lang:"csharp"](code/cs/TreeMdAdditional.cs)
{% sample lang="c" %}
[import:6-10, lang:"c_cpp"](code/c/Tree_example.c)
{% sample lang="js" %}
[import:3-8, lang:"julia"](code/julia/Tree.jl)
{% sample lang="py2" %}
[import:1-5, lang:"python"](code/python2/Tree_example.py)
{% sample lang="py3" %}
[import:5-10, lang:"python"](code/python3/Tree_example.py)
{% sample lang="scratch" %}
[import:3-8, lang:"julia"](code/julia/Tree.jl)
{% endmethod %}

Because of this, the most straightforward way to traverse the tree might be recursive. This naturally leads us to the Depth-First Search (DFS) method:

{% method %}
{% sample lang="jl" %}
[import:11-18, lang:"julia"](code/julia/Tree.jl)
{% sample lang="cpp" %}
[import:6-13, lang:"c_cpp"](code/c++/tree.cpp)
{% sample lang="cs" %}
[import:48-57, lang:"csharp"](code/cs/TreeMdAdditional.cs)
{% sample lang="c" %}
[import:81-92, lang:"c_cpp"](code/c/Tree_example.c)
{% sample lang="js" %}
[import:15-23, lang:"javascript"](code/javascript/Tree_example.js)
{% sample lang="py2" %}
[import:8-16, lang:"python"](code/python2/Tree_example.py)
{% sample lang="py3" %}
[import:10-18, lang:"julia"](code/julia/Tree.jl)
{% sample lang="scratch" %}
[import:10-18, lang:"julia"](code/julia/Tree.jl)
{% endmethod %}

At least to me, this makes a lot of sense. We fight recursion with recursion! First, we first output the node we are on and then we call `DFS_recursive(...)` on each of its children nodes. This method of tree traversal does what its name implies: it goes to the depths of the tree first before going through the rest of the branches. In this case, the ordering looks like:

![DFS ordering pre](res/DFS_pre.png)

Note that the in the code above, we are missing a crucial step: *checking to see if the node we are using actually exists!* Because we are using a vector to store all the nodes, we will be careful not to run into a case where we call `DFS_recursive(...)` on a node that has yet to be initialized; however, depending on the language we are using, we might need to be careful of this to avoid recursion errors! 

Now, in this case the first element searched through is still the root of the tree. This type of tree traversal is known as *pre-order* DFS. We perform an action (output the ID) *before* searching through the children. If we shift the function around and place the data output at the end of the function, we can modify the order in which we search through the tree to be *post-order* and look something like this:


{% method %}
{% sample lang="jl" %}
[import:21-29, lang:"julia"](code/julia/Tree.jl)
{% sample lang="cpp" %}
[import:15-22, lang:"c_cpp"](code/c++/tree.cpp)
{% sample lang="cs" %}
[import:75-84, lang:"csharp"](code/cs/TreeMdAdditional.cs)
{% sample lang="c" %}
[import:20-29, lang:"julia"](code/julia/Tree.jl)
{% sample lang="js" %}
[import:20-29, lang:"julia"](code/julia/Tree.jl)
{% sample lang="py2" %}
[import:20-29, lang:"julia"](code/julia/Tree.jl)
{% sample lang="py3" %}
[import:20-29, lang:"julia"](code/julia/Tree.jl)
{% sample lang="scratch" %}
[import:20-29, lang:"julia"](code/julia/Tree.jl)
{% endmethod %}

![DFS ordering post](res/DFS_post.png)

In this case, the first node visited is at the bottom of the tree and moves up the tree branch by branch. In addition to these two types, binary trees have an *in-order* traversal scheme that looks something like this:

{% method %}
{% sample lang="jl" %}
[import:32-47, lang:"julia"](code/julia/Tree.jl)
{% sample lang="cpp" %}
[import:25-37, lang:"c_cpp"](code/c++/tree.cpp)
{% sample lang="cs" %}
[import:86-104, lang:"csharp"](code/cs/TreeMdAdditional.cs)
{% sample lang="c" %}
[import:31-47, lang:"julia"](code/julia/Tree.jl)
{% sample lang="js" %}
[import:31-47, lang:"julia"](code/julia/Tree.jl)
{% sample lang="py2" %}
[import:31-47, lang:"julia"](code/julia/Tree.jl)
{% sample lang="py3" %}
[import:31-47, lang:"julia"](code/julia/Tree.jl)
{% sample lang="scratch" %}
[import:31-47, lang:"julia"](code/julia/Tree.jl)
{% endmethod %}

![DFS ordering in](res/DFS_in.png)

The order here seems to be some mix of the other 2 methods and works through the binary tree from left to right.

Now, at this point, it might seem that the only way to search through a recursive data structure is with recusion, but this is not necessarily the case! Rather surprisingly, we can perform a DFS non-recursively by using a stack, which are data structures that hold multiple elements, but only allow you to interact with the very last element you put in. The idea here is simple:

1. Put the root node in the stack
2. Take it out and put in its children
3. Pop the top of the stack and put its children in
4. Repeat 3 until the stack is empty

In code, it looks like this:

{% method %}
{% sample lang="jl" %}
[import:50-61, lang:"julia"](code/julia/Tree.jl)
{% sample lang="cpp" %}
[import:39-52, lang:"c_cpp"](code/c++/tree.cpp)
{% sample lang="cs" %}
[import:36-52, lang:"csharp"](code/cs/Tree.cs)
{% sample lang="c" %}
[import:18-20, lang:"c_cpp"](code/c/Tree_example.c)
[import:37-48, lang:"c_cpp"](code/c/Tree_example.c)
[import:94-113, lang:"c_cpp"](code/c/Tree_example.c)
{% sample lang="js" %}
[import:25-40, lang:"javascript"](code/javascript/Tree_example.js)
{% sample lang="py2" %}
[import:25-36, lang:"python"](code/python2/Tree_example.py)
{% sample lang="py3" %}
[import:31-45, lang:"python"](code/python3/Tree_example.py)
{% sample lang="scratch" %}
[import:49-61, lang:"julia"](code/julia/Tree.jl)
{% endmethod %}

All this said, there are a few details about DFS that might not be idea, depending on the situation. For example, if we use DFS on an incredibly long tree, we will spend a lot of time going further and further down a single branch without searching the rest of the data structure. In addition, it is not the natural way humans would order a tree if asked to number all the nodes from top to bottom. I would argue a more natural traversal order would look something like this:

![BFS ordering](res/BFS_simple.png)

And this is exactly what Breadth-First Search (BFS) does! On top of that, it can be implemented in the same way as the `DFS_stack(...)` function above, simply by swapping the `stack` for a `queue`, which is similar to a stack, exept that it only allows you to interact with the very first element instead of the last. In code, this looks something like:

{% method %}
{% sample lang="jl" %}
[import:64-75, lang:"julia"](code/julia/Tree.jl)
{% sample lang="cpp" %}
[import:54-67, lang:"c_cpp"](code/c++/tree.cpp)
{% sample lang="cs" %}
[import:54-70, lang:"csharp"](code/cs/Tree.cs)
{% sample lang="c" %}
[import:115-135, lang:"c_cpp"](code/c/Tree_example.c)
{% sample lang="js" %}
[import:42-57, lang:"javascript"](code/javascript/Tree_example.js)
{% sample lang="py2" %}
[import:38-49, lang:"python"](code/python2/Tree_example.py)
{% sample lang="py3" %}
[import:48-62, lang:"python"](code/python3/Tree_example.py)
{% sample lang="scratch" %}
[import:63-75, lang:"julia"](code/julia/Tree.jl)
{% endmethod %}

# Example Code
{% method %}
{% sample lang="jl" %}
### Julia
[import, lang:"julia"](code/julia/Tree_example.jl)
{% sample lang="cpp" %}
### C++
[import, lang:"c_cpp"](code/c++/tree_example.cpp)
{% sample lang="cs" %}
### C# #
[import, lang:"csharp"](code/cs/Tree.cs)
{% sample lang="c" %}
### C
[import, lang:"c_cpp"](code/c/Tree_example.c)
{% sample lang="js" %}
### JavaScript
[import, lang:"javascript"](code/javascript/Tree_example.js)
{% sample lang="py2" %}
### Python 2
[import, lang:"python"](code/python2/Tree_example.py)
{% sample lang="py3" %}
### Python 3
[import, lang:"python"](code/python3/Tree_example.py)
{% sample lang="scratch" %}
### Scratch
![scratch tree](code/scratch/scratch_tree.png)
{% endmethod %}
