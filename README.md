# Specific presentation of task formats, generated prompts and discussion examples about ModalLogicBench

**We provide examples of mBD,mDS and mPC in the code section for reference！**

## 1. Tash Formats

### 1.1 Examples of original and variant questions for the mBD rule in ModalLogicBench_BQA.

We use mBD (□(p→r),□(q→s),□p∨□¬s ⊢ □r∨□¬q) as an example and compose the other three variant questions with different state combinations of propositions r and q (□¬r∨□¬q，□¬r∨□q, and □r∨□q).

 Propositions and questions of mBD  | Natural language statement  
 ---- | ----- 
Propositions  | (a) It must be the case that the system has become operational, (b) It must not be the case that Liam drafts the project proposal.
Original Question  | Can we say that at least one of the following (a) and (b) is always true? (a) It must be the case that the system has become operational, (b) It must not be the case that Liam drafts the project proposal.
Variant Question1  | Can we say that at least one of the following (a) and (b) is always true? (a) It must not be the case that the system has become operational, (b) It must not be the case that Liam drafts the project proposal. 
Variant Question2  | Can we say that at least one of the following (a) and (b) is always true? (a) It must be the case that the system has become operational, (b) It must be the case that Liam drafts the project proposal. 
Variant Question3  | Can we say that at least one of the following (a) and (b) is always true? (a) It must not be the case that the system has become operational, (b) It must be the case that Liam drafts the project proposal. 

### 1.2 Examples of questions and choices for the mBD rule in ModalLogicBench_MCQA

Let's take mBD as an example, where choice_3 is the correct option.

 Propositions and questions of mBD  | Natural language statement  
 ---- | -----   
  Context  | If it is necessarily the case that Emma completes the software update, then the system must necessarily become operational. If it is necessarily the case that Liam drafts the project proposal, then the proposal must necessarily get submitted. It is known that either Emma necessarily completes the software update or the proposal is necessarily not submitted. It is uncertain which statement is correct, or whether both are correct.    
 Question  | What would be the most appropriate conclusion based on the given context?  
 Choice_1  | It must be the case that the proposal is submitted or it must not be the case that Emma completes the software update. 
Choice_2  | It must be the case that the system has become operational or it must not be the case that John completes the software update.
Choice_3  | It must be the case that the system has become operational or it must not be the case that Liam drafts the project proposal.
Choice_4  | It must be the case that the system has become operational or it must not be the case that Sarah reviews the project proposal. 

## 2. Key prompts  for ModalLogicBench

### 2.1  Prompts for Sentence Generation

The sentence generation schema consists of three main modules: the task definition, the example sentence, and the format instruction. The task definition provides a natural language representation of the rules from the Rule2NL part. The example sentence helps the generative model understand the rule's meaning and illustrates the format for the third part. The format instruction guides the model to generate sentences in the correct structure. For example, the mMiN rule, ◇¬p ⊢ ¬□p, translates to “If ‘probably not p’ is true, then we can infer ‘not necessarily p’.” .
The specific process is shown in the following figure.
![image](https://github.com/user-attachments/assets/b2dc26ab-aa68-4a83-84b8-ae5c0661ec47)

### 2.2  Task MCQA(Question selection and incorrect generation)

Candidate questions for the MCQA task are randomly selected from the following three questions.

1.	What would be the most appropriate conclusion based on the given context?
	
2.	In light of the provided context, what conclusion can be considered the most ap-propriate?

3.	Based on the context, what conclusion would be deemed most suitable?

The generated prompt regarding the MCQA incorrect option is shown in the following figure.
![image](https://github.com/user-attachments/assets/61abb4ed-d6c7-439e-a2df-751a5cfc2137)


### 2.3  Prompt for Task BQA and MCQA Evaluation

For BQA and MCQA tasks, the zero-shot CoT format differs between the two. BQA involves determining whether a question is true or false, so the input requires only the context, question, and CoT prompt. In contrast, for MCQA, you also need to provide options. We have defined three different prompt strategies for each task.

![image](https://github.com/user-attachments/assets/72514635-30cd-44d4-bfdb-087ef38e9eb9)


## 3. Discussion Examples
### 3.1  The Example of Process Fallacies

We use the mBD rule to illustrate the process fallacy phenomenon.

The mBD criterion has the following form:  □(p→r),□(q→s),□p∨□-s=> □r∨□-q,  and its natural language representation is “If it is necessarily the case that if p holds, then r must hold,  and it is necessarily the case that if q holds, then s must hold,  and it is necessarily the case that either p holds or s is false,  then it must necessarily be the case that either r holds or q is false.” We analyze the reasoning processes of GPT-4o  mini qwq-32b-preview,  and DeepSeek-R1 for this criterion. Although all three models reach the final conclusion of “yes”,  GPT-4o mini only performs semantic deduction and considers only a subset of possible cases. In contrast,  DeepSeek-R1 accounted for all aspects but failed to explicitly indicate modal operators during symbolic transformation.  Meanwhile,  qwq-32b-preview performed a more rigorous and comprehensive reasoning process. The specific context and question are  shown in Table 1, and the detailed reasoning procedure is shown in Table 2.

**Table 1**
Context/Question  | Natural language statement 
 ---- | -----  
Context  | If it is necessarily the case that Noah trains for the marathon, then he must necessarily build physical endurance. If it is necessarily the case that Mia prepares healthy meals, then she must necessarily feel energized. It is understood that either Noah necessarily trains for the marathon or Mia necessarily does not feel energized. It is unclear which statement is accurate, or whether both are accurate.
Question | Can we say that at least one of the following (a) and (b) is always true? (a) It must be the case that Noah builds physical endurance, (b) It must not be the case that Mia prepares healthy meals. 

**Table 2**
 Model Name  | Reasoning Steps
 ---- | -----   
GPT4omini  | 1. The context establishes that if Noah trains for the marathon, he must build physical endurance.2. It is also established that if Mia prepares healthy meals, she must feel energized.3. We know that either Noah necessarily trains for the marathon, or Mia necessarily does not feel energized.4. If Noah does not train for the marathon, then Mia must not feel ener-gized, which implies Mia does not prepare healthy meals.5. Therefore, it is always true that at least one of the following must hold:- (a) Noah builds physical endurance (if he trains for the mara-thon).- (b) Mia does not prepare healthy meals (if she does not feel en-ergized).
qwq-32b-preview  | 1. Let's break down the given statements and see what they imply.2. First, "if it is necessarily the case that Noah trains for the marathon, then he must necessarily build physical endurance." this can be repre-sented as: □n → □e, where n is "Noah trains for the marathon" and e is "Noah builds physical endurance."3. Second, "if it is necessarily the case that Mia prepares healthy meals, then she must necessarily feel energized." this can be represented as: □m → □f, where m is "Mia prepares healthy meals" and f is "Mia feels en-ergized."4. Third, "it is understood that either Noah necessarily trains for the marathon or Mia necessarily does not feel energized." this can be repre-sented as: □n ∨ □¬f.5. We need to determine if at least one of the following is always true: (a) it must be the case that Noah builds physical endurance (□e), or (b) it must not be the case that Mia prepares healthy meals (□¬m).6. So, we need to see if □e ∨ □¬m is always true given the previous statements.7. Let's consider the possible scenarios. 8. Scenario 1: □n is true. a. from □n → □e, if □n is true, then □e must be true. b. therefore, in this scenario, □e is true, so □e ∨ □¬m is true.9. Scenario 2: □n is false. a. from □n ∨ □¬f, if □n is false, then □¬f must be true. b. so, □¬f is true, which means it is necessarily the case that Mia does not feel energized. c. now, consider □m → □f. d. if □m is true, then □f must be true. e. but we have □¬f is true, which contradicts □f.f. therefore, □m cannot be true;  otherwise, we have □f and □¬f, which is a contradiction. g. hence, □¬m must be true. h. therefore, in this scenario, □¬m is true, so □e ∨ □¬m is true.10. In both possible scenarios, □e ∨ □¬m is true.11. Therefore, we can say that at least one of (a) or (b) is always true.
DeepSeek-R1(the thought process is removed) | 1. Premises: N → B, M → E, and N ∨ ¬E.2. If N is true, B must hold (from N→B), making (a) true.3. If N is false, ¬E must hold (from N∨¬E).4. ¬E implies ¬M (contrapositive of M→E), making (b) true.5. Thus, in all scenarios, at least (a) or (b) is necessarily true.
