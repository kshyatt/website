
# 📚 Tutorials


### Fundamentals

* [[Understanding Tensor Diagrams|tutorials/diagrams]]

  Tensor network diagrams are a powerful way to express 
  contractions of many tensors.
  Learn to understand tensor diagrams and translate them 
  into ITensor code.

* [[Estimating the Cost of Evaluating a Tensor Network|tutorials/cost]]

  We discuss how to determine the cost of evaluating a tensor network and best practices for 
  computing properties of matrix product states.

* [[Singular Value Decomposition|tutorials/SVD]]
  
  The singular value decomposition (SVD) provides a way to separate large degrees 
  of freedom from irrelevant ones.  ITensors allows for the easy SVD of tensors.


### ITensor Library Features

* [[Priming Indices in ITensor|tutorials/primes]]

  ITensor uses a flexible priming system to prevent indices from automatically 
  contracting.  We discuss best practices and give examples.

<!-- Commented out for the time being
* [[The Matrix Product State (MPS)|tutorials/MPS]]

  ITensor includes a full-featured matrix product state class that can be used
  with or without quantum number conservation.
  -->

* [[Matrix Product Operators (MPO)|tutorials/MPO]]

  The Hamiltonian may be rewritten in terms of a site by site operator.  This 
  article provides an overview

* [[AutoMPO|tutorials/AutoMPO]]

  Instead of making MPOs by hand, ITensor allows for the automatic creation of MPOs in only a few lines of code.


* [[Calculating a Two-Site Correlation Function|tutorials/correlations]]

  Contracting a tensor network to measure a two-operator correlation
  function from an MPS is shown in diagrammatic form with ITensor code.

### C++ Tutorials

* [[The "Args" Named Arguments System|tutorials/args]]

  Args is a system used in ITensor to pass named parameters to functions,
  and can be a useful addition to your own code.
  Examples of named arguments include SVD accuracy parameters ("Maxm", "Cutoff")
  and parameters controlling the amount of information printed by an algorithm. 

* [[Git Quickstart Guide|tutorials/git]]

  Git is the version control system used to maintain ITensor.
  Learn the basic git workflow and how to contribute to the ITensor code base.


<br/>