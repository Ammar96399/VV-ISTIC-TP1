# Practical Session #1: Introduction

1. Find in news sources a general public article reporting the discovery of a software bug. Describe the bug. If possible, say whether the bug is local or global and describe the failure that manifested its presence. Explain the repercussions of the bug for clients/consumers and the company or entity behind the faulty program. Speculate whether, in your opinion, testing the right scenario would have helped to discover the fault.

2. Apache Commons projects are known for the quality of their code and development practices. They use dedicated issue tracking systems to discuss and follow the evolution of bugs and new features. The following link https://issues.apache.org/jira/projects/COLLECTIONS/issues/COLLECTIONS-794?filter=doneissues points to the issues considered as solved for the Apache Commons Collections project. Among those issues find one that corresponds to a bug that has been solved. Classify the bug as local or global. Explain the bug and the solution. Did the contributors of the project add new tests to ensure that the bug is detected if it reappears in the future?

3. Netflix is famous, among other things we love, for the popularization of *Chaos Engineering*, a fault-tolerance verification technique. The company has implemented protocols to test their entire system in production by simulating faults such as a server shutdown. During these experiments they evaluate the system's capabilities of delivering content under different conditions. The technique was described in [a paper](https://arxiv.org/ftp/arxiv/papers/1702/1702.05843.pdf) published in 2016. Read the paper and briefly explain what are the concrete experiments they perform, what are the requirements for these experiments, what are the variables they observe and what are the main results they obtained. Is Netflix the only company performing these experiments? Speculate how these experiments could be carried in other organizations in terms of the kind of experiment that could be performed and the system variables to observe during the experiments.

4. [WebAssembly](https://webassembly.org/) has become the fourth official language supported by web browsers. The language was born from a joint effort of the major players in the Web. Its creators presented their design decisions and the formal specification in [a scientific paper](https://people.mpi-sws.org/~rossberg/papers/Haas,%20Rossberg,%20Schuff,%20Titzer,%20Gohman,%20Wagner,%20Zakai,%20Bastien,%20Holman%20-%20Bringing%20the%20Web%20up%20to%20Speed%20with%20WebAssembly.pdf) published in 2018. The goal of the language is to be a low level, safe and portable compilation target for the Web and other embedding environments. The authors say that it is the first industrial strength language designed with formal semantics from the start. This evidences the feasibility of constructive approaches in this area. Read the paper and explain what are the main advantages of having a formal specification for WebAssembly. In your opinion, does this mean that WebAssembly implementations should not be tested? 

5.  Shortly after the appearance of WebAssembly another paper proposed a mechanized specification of the language using Isabelle. The paper can be consulted here: https://www.cl.cam.ac.uk/~caw77/papers/mechanising-and-verifying-the-webassembly-specification.pdf. This mechanized specification complements the first formalization attempt from the paper. According to the author of this second paper, what are the main advantages of the mechanized specification? Did it help improving the original formal specification of the language? What other artifacts were derived from this mechanized specification? How did the author verify the specification? Does this new specification removes the need for testing?

## Answers

1. Boeing's utilization of the Maneuvering Characteristics Augmentation System (MCAS) as a flight control software solution for its 737 MAX aircraft has been widely acknowledged as a significant contributing factor to the occurrence of two crashes that resulted in the tragic loss of 346 lives. On October 29th, 2018, the first of two crashes occurred, followed by a second on March 10th, 2019. 

    The Maneuvering Characteristics Augmentation System (MCAS), a flight stabilization feature developed by Boeing, was identified as the root cause of both crashes. MCAS was designed to adjust the nose of the aircraft based on data collected from two angle-of-attack sensors, one located on each side of the plane, in order to maintain stability during flight. The critical flaw in the MCAS system was that it relied solely on one of the two angle-of-attack sensors to make its calculations. As a result, if the data from this single sensor was incorrect, there was no redundancy in place to detect or correct the error, potentially leading to a dangerous and unstable flight situation. 
    
    This was a very coslty bug which could have been avoided by more testing and less trusting. The failure of the MCAS system was a costly bug that could have been prevented through more rigorous testing and a more cautious approach to development. A lack of adequate testing and overreliance on the single sensor data contributed to the tragic outcomes of the crashes, highlighting the importance of comprehensive testing and redundancy in critical systems.

2. The bug COLLECTIONS-692 is a local bug. The function ``createSetBasedOnList(final Set<E> set, final List<E> list)`` should create and return a new set with the same type as the provided ``set`` and populate it with all the elements of ``list``. It appears that a commit removed the line of code responsible for populating the new set with the elements of the list. Unfortunately, due to a lack of testing for this function, the error went unnoticed. To prevent similar issues from occurring in the future, the contributor has added a new test for this specific method.

3. Concrete experiments: Could Netflix's fallback system works well to minimalize the impacts on user experience in case of service degradation? 

   Requirements: Hypothesis, independent and dependent variables as well as context should be specified.

   Variables: How many users start streaming a video each second.

   Results: Some finer-grained metrics were to be observed when the system was not working properly, even if that isn't felt by the user.

   The only: no.

   How could be carried in other organisations: A search in Google Scholar game several interesting results such like [a chaos engineering example in the JVM](https://ieeexplore.ieee.org/abstract/document/8908767) and [an analysis on chaos engineering for docker containers](https://www.sciencedirect.com/science/article/abs/pii/S0167739X21001163). While these papers didn't indicate companies, they showed how the chaos engineering could be used in industrial production ready projects.

   A possible scenario of chaos engineering could be compiler optimisation where such optimisation features could be injected with observable metrics like "the ratio of lines between source code and binary codes during different compiling steps" to observe the performance that are related to minor but critical changes. Such results could be precious for further compiler developments or even direct to new features to be proposed. However, this practice could be controversing like how telemetries were introduced in languages like Go and were criticized by the community.

5. Every programming language needs a formal specification. The formal specification contains sets of rules to define the syntax of a language, how this language is going to be checked against the type system, and how types of different structures are inferred.

   The most interest of this specification is that it's defined with a mathematical language that shared in computer science so it provide a basis that researchers can test against to see its soundness and such soundness is proved with a mathematical foundation.

   There are also other advantages including:

   -   Providing an insight to be shared in the developers and engineers of this language, in a formal way, to eliminate most of misunderstandings. An infamous counter example is JavaScript.
   -   Providing a set of mathematical properties that could be analysed with mathematical toolchains including specifical methods or proof assistants.
   -   With a formal specification, implementing automation toolchain such as type checker or static analyser could be relatively feasible for anyone with a computer science background and the correctness of these toolchains could be further checked against these specifications.

   This does not means that implementations should not be tested. In fact, formal specification could not be determined as correct without conducting formal verification or testing. Therefore it is always a good practice to accompany the formal specification with several test sets, from small syntactic structure to several sample codes of complex programs, as well as unit tests for behaviors in different level in different components in the implementation. Testing remains much less expensive than formal methods, and well designed tests could be good documentation for compiler developers as well as language users.

5. This is an example of how the researchers could test the soundness of a language in the previous question.

   According to the author, the mechanised specification allows to find issues in the specification, therefore ensures the soundness of this specification.

   Yes, according to the paper, several uncovered issues were found in the specification, which means that the initial specification was not sound. The WASM designers were therefore able to correct their typing system.

   The executables of a verified interpreter and a type checker were derived from this mechanised specification.

   Although the formal verification eliminates the need of testing (since the correctness of artefact was mathematically proved), the code for the proof is difficult to be understood for most of the audience. Testing could provide documentary supports for the community for better understanding of the specification and gives a prehook style checking for incoming feature requests.
