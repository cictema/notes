# InvsCov : [The Use of Likely Invariants as Feedback for Fuzzers](https://github.com/eurecom-s3/invscov#the-use-of-likely-invariants-as-feedback-for-fuzzers)


From Github: "This prototype implements the idea described in our [USENIX Security '21 paper](https://www.usenix.org/conference/usenixsecurity21/presentation/fioraldi), a new feedback mechanism that augments code coverage by taking into account the usual values and relationships among program variables.

One of the main limitations of coverage-guided fuzzing is the fact that most of the available solutions are optimized to reach different parts of the program under test, but struggle when reachability alone is insufficient to trigger a vulnerability. In reality, many bugs require a specific program state that involves not only the control flow, but also the values of some of the program variables.

For this purpose, we learn likely invariants over variables at the basic- block level, and partition the program state space accordingly. Our feedback can distinguish when an input violates one or more invariants and reward it, thus refining the program state approximation that code coverage normally offers.

This prototype, called InvsCov, is based on AFL++, LLVM and the Daikon invariant detector."

#  Key Points:

1. **Objective**: The paper aims to enhance the effectiveness of fuzz testing by considering not only code coverage but also local characteristics of program states. They seek to identify and leverage likely invariants in program variables.

2. **Feedback for Fuzz Testing**: The proposed approach introduces a new feedback mechanism for fuzz testing. In addition to code coverage, the feedback mechanism takes into account local divergences from typical variable values during program execution.
   
3. **Mining Likely Invariants**: To gather information about these likely invariants, the authors execute an input corpus (a set of test inputs) and analyze the program's behavior. They mine constraints on the values and relationships of program variables from these executions. These constraints represent local characteristics of the input corpus.
   
4. **Not Necessarily Program Properties**: The paper emphasizes that the constraints obtained through execution-based invariant mining may not necessarily represent general properties of the program but rather local characteristics of the input corpus. This means that these constraints can be violated under different inputs.
   
5. **Feedback Function**: A new feedback function is defined to consider these likely invariants when assessing code coverage. This function treats an edge (a transition in the program) differently if it encounters variable values that violate a likely invariant. This approach aims to increase the sensitivity of the fuzzing system.
   
6. **Implementation**: The authors implement their approach in a prototype called INVSCOV, built on top of LLVM and the AFL++ fuzzer. LLVM is a popular compiler infrastructure, and AFL++ is an extended version of the American Fuzzy Lop (AFL) fuzzer, which is known for its effectiveness in finding security vulnerabilities.
   
7. **Performance Overhead**: To ensure practicality and efficiency, the authors mention that they have developed techniques to instrument programs with low performance overhead. This is crucial in the context of fuzzing, where efficiency is a key concern.
   
8. **Experimental Results**: The paper reports the results of experiments conducted on a set of programs that are commonly tested by other fuzzers. The results suggest that their feedback mechanism, which considers information about program state in addition to control flows, can uncover more and different software bugs compared to traditional coverage-guided fuzzing (CGF) approaches. 

#  Why Invariants?

1. **Invariant-Driven Testing**: Invariants provide a foundation for defining expected program behaviors, making them a natural choice for guiding the testing process.

2. **Error Detection**: Invariants can serve as correctness oracles, allowing the detection of deviations from expected program properties and, by extension, the identification of errors or vulnerabilities.
   
3. **Diverse Test Case Generation**: Invariants help generate diverse test cases by focusing on behaviors that deviate from expected program state properties, leading to a broader exploration of potential issues.
   
4. **Leveraging Dynamic Analysis**: Dynamic invariants derived from program execution provide more precise and numerous invariants, enabling the identification of local program properties and behaviors.
   
5. **Coverage Enhancement**: Using invariants as heuristics can improve the coverage of the program's behaviors, making the testing process more effective in uncovering a wider range of software bugs and vulnerabilities. 

# Daikon

[Daikon](http://plse.cs.washington.edu/daikon/) is an implementation of dynamic detection of likely invariants; that is, the Daikon invariant detector reports likely program invariants. It is used by InvsCov to mine likely  invariants from programs to instrument them to expose violation of invariants.

Daikon expects two files to begin mining invariants:
- `.dtrace` file: This file contains a trace of a particular execution of your program. It records the program's behavior during that specific run.
- `.decls` file: This file contains information about the variables and functions present in your program. It also provides information about grouping the variables into abstract types.

These files are created by a front-end such as Kvasir for C/C++ files. However, for InvsCov, the  `.decls`  file is produced by `dump-cc` and `reconstruct-dump`, and the `.dtrace` files are created by Kvasir under the hood as Daikon incrementally mines invariants when we run `learn-invariants`.

#### Invariant Data Structure:
1. Invariants are printed to the terminal, and,
2. Stored as binary files with the extension `.inv.gz`.
3. These `.inv.gz` files are simply a serialized [`PptMap`](http://plse.cs.washington.edu/daikon/download/api/daikon/PptMap.html) object in Java. Thus, further tools can be written to read, analyze, edit the invariants.

#### Daikon Tools to Process Invariants:
1. **`MergeInvariants`**: 
	1. The `MergeInvariants` utility is a tool provided by Daikon for merging multiple serialized invariant files into a single serialized invariant file. This merged file contains invariants that are true across each of the input files. In essence, it consolidates invariant information from multiple sources into a single, comprehensive result.
	2. The primary purpose of `MergeInvariants` is to consolidate and merge invariant information from multiple sources, such as different runs of Daikon on various input data files. The result is a single file that contains invariants that hold true across all the input files, providing a more comprehensive understanding of the program's behavior. This merged invariant file can be useful for post-processing, comparison, and further analysis.
2. **`Invariant Diff`**:
	1. The `Invariant Diff` utility is a tool provided by Daikon for comparing and analyzing the differences between two sets of invariants. It's useful for examining how the invariants generated by different versions of a program or different runs of the same program compare to each other. This comparison can help identify changes or anomalies in program behavior.
	2. The `Invariant Diff` utility is a valuable tool for analyzing the differences between sets of invariants, helping you understand how program behavior has changed or diverged between different versions or runs. This can be useful for identifying potential issues or tracking the impact of program modifications.
3. **`PrintInvariants`** : Print invariants in text from a binary `.inv.gz` file.
# Implementation

### Learning Phase:

- **Objective**: The Learning phase is primarily focused on collecting information about the program's state that is necessary for invariant mining. This information is crucial for defining the likely invariants used to drive the fuzzing process.
   
- **Instrumentation for Data Collection**: During this phase, the authors instrument the target program to capture the state of program variables as it executes. This involves adding additional code to the program under test. The instrumented code logs information related to the program state, such as the values of variables, data structures, or other relevant runtime data.
   
- **Input Corpus**: To gather this information, an augmented version of the program under test is executed over a corpus of input data. The input corpus represents a set of test cases, which can be obtained through various means. In the experiments described, the authors mention using seeds generated from a 24-hour coverage-guided fuzzing session. These seeds serve as the initial set of inputs for the program.
   
- **Invariant Mining Tool (Daikon)**: The data collected during the Learning phase is used for invariant mining. The authors mention using the Daikon dynamic invariant detector, which is a tool for extracting invariants from program executions. It analyzes the recorded program behavior to identify likely invariants.
   
- **Computational Complexity**: Invariant mining can be computationally intensive. In general, the complexity of invariant mining is cubic in the number of program variables. However, because the approach operates at the level of basic blocks, the number of variables considered is relatively small, resulting in a linear computation cost concerning the number of basic blocks in the program.
   
#### Steps for Learning Phase:

1. **Preprocessing**:
	1. Set environment variables.
	2. **Compile the Program with `dump-cc` and `dump-c++`**:    
	    - First, navigate to the source code of the target program (`target_program_src/`).	        
	    - Use the `./configure` command to configure the program for compilation.	        
	    - The `make` command is used for compiling the program. However, in this case, the `CC` and `CXX` environment variables are set to point to `dump-cc` and `dump-c++`, respectively. These are specialized compilers provided by InvsCov for instrumenting the program.
	3. **`dump-cc`**:  dump-cc compiles the PUT (Program Under Test) with `clang-10`, using a custom LLVM pass named `dump-pass.so`:
		- The LLVM function pass instruments the program with logging mechanisms. 
		- **Dumping Comparability Sets and Integer Ranges**:    
			- In addition to logging variable values, the function pass also collects information about comparability sets and integer ranges. These sets and ranges describe how variables relate to each other and the bounds within which integer values can vary.
		- **JSON Files for Code Modules**:    
			- The function pass creates separate JSON files for each code module within the program. These JSON files store information about various aspects of the program, including program points, variable types, comparability information, and integer bounds.			   ![[json_files.png]]
	1. **`reconfigure-dump`: Processing and Merging Intermediate Files**:
			- The JSON files generated for each code module contain intermediate information about the program's state and variables. These files are processed and merged by consolidating information from different modules into a unified representation of the program's state and variables to singular `.decls` file for Daikon. This file includes comprehensive information about program points, variable types, comparability sets, and integer bounds. It forms a structured representation of the program's state.
		 ![[decls_file 1.png]]
        1. After compiling the program using `dump-cc` and `dump-c++`, a new binary called `program_dump` is created. This binary is the instrumented version of the program, prepared for further analysis.

2. **Learn Invariants**: 
	1. The `learn-invariants` tool uses the initial corpus and the instrumented program to monitor the program's behavior and extract likely invariants. These invariants will guide the subsequent fuzz testing.
	2. **Run Learn-Invariants with the Dumper Binary**:
	   - In this step, the `learn-invariants` tool is executed to learn likely invariants from the instrumented program (`program_dump`).
	   - The command expects the following arguments:
	     - `/path/to/initial_corpus`: the queue of a coverage-guided fuzzer taken as soon as that fuzzer shows signs of slowing down in reaching new coverage points. A violation of an invariant learned over such corpus will lead to novel feedback for the fuzzer and desaturate the search.
	     - `./program_dump`: path to the instrumented program.
	     - `@@`: placeholder for the corpus files.
	 3. **Modified Daikon**: 
		 - Under the hood, `learn-invariants` simply calls a modified version of Daikon to run over the set of corpus files and learn invariants incrementally.
		 - In the modified Daikon, authors added a `--corpus` mode, where instead of a single `.dtrace` file generated by Kvasir, Daikon runs over the corpus of seeds, with the logging instrumentation done by the LLVM pass generating the `.dtrace` files.
		 - Daikon then runs over the `.dtrace` files in an `on-demand` mode to generate invariants incrementally.
### Instrumentation Phase: 

- **Objective**: The Instrumentation phase builds on the information gathered in the Learning phase and prepares the program for coverage-guided fuzzing. In this phase, the collected likely invariants are incorporated into the program to guide the fuzzing process.   
- **Augmenting Program Code**: During this phase, the authors modify the code of the program under test. They introduce changes that allow the program to evaluate the likely invariants identified in the Learning phase. These likely invariants reflect the observed program behaviors and properties.
#### Steps for Instrumentation Phase:

1. **Run Generate-Constraints**:
   - After learning likely invariants (post-daikon), the `generate-constraints` tool is executed. 
   - Turns each invariant into a C function.
   - These functions are compiled to LLVM IR which are to be invoked from the program point of interest.
   - These functions take as arguments the IR values that are part of the invariant and evaluate them, returning a unique identifier when the invariant is violated, and zero otherwise.
2.  **`instrument-cc`**:
	1. Compile the main program (not the dumper binary) with `instrument-cc`, which compiles the program using clang with various passes (`afl-llvm-pass.so` by default, and `cmplog-routines-pass.so`,`split-switches-pass.so`,`cmplog-instructions-pass.so` if arg `CMPLOG` is true, and `split-switches-pass.so`, `compare-transform-pass.so`, `split-compares-pass.so` if arg `LAF_INTEL` is set), along with ASAN, UBSAN, and MSAN flags, if set.

#### Then, this binary is fuzzed by afl.

In summary, the Learning phase involves instrumenting the program to capture runtime information, while the Instrumentation phase modifies the program code to incorporate the identified likely invariants and make it compatible with coverage-guided fuzzers. These two phases are integral to the overall approach, providing the necessary foundation for effective invariant-based fuzz testing.


## Our Approach:

As we want to fuzz by incrementally finding new coverage increasing seeds, we would be following these steps:
1. **Preprocess the Binary:**  
	1. Create a ".decls" file. 
	2. Create a dumper binary.
2. **Instrument the Program Under Test (PUT).**
3. **Start Fuzzing**
4. **Upon Finding a New Coverage-Increasing Seed:** 
	1. Copy the fuzz state of the PUT. 
	2. Terminate the fuzzing process. 
	3. Use Daikon with the new seed to discover new invariants.
5. **When New Invariants Are Found:** 
	1. Merge these new invariants with the existing set of invariants using a tool like "MergeInvariants."
6. **Reinstrument the Program with the Updated Invariants.**
7. **Resume the Fuzzing Process.**

### Flowchart

![[OurApproach.png]]


### Key Points of Our Approach:

1. **Data Structure Modification:**
	1. The primary data structure being modified is the Java `PptMap`, which stores invariants.
	2. `PptMap` is stored in binary `.inv.gz` files.
2. **Merging Invariants:**
	1. Invariants can be merged using Daikon's `MergeInvariants` tool.
	2. Custom Java tools can be developed for merging, allowing flexibility in defining merging rules.
3. **Alternative to Merging Invariants:**
	1. Instead of merging the entire set of previous invariants, new constraints derived from newly found invariants can be considered.
	2. This approach may introduce duplicates of existing invariants.
	3. These new constraints can be instrumented directly into the program alongside previously generated invariants.
4. **Cost Calculation:**
	1. To optimize the approach, calculate the costs of specific operations.
	2. These calculations should include: 
		1. Running Daikon on a single seed for a large program. 
		2. The computational cost of merging invariants. 
		3. The resource cost of re-instrumenting the program with new invariants.