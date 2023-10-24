# rust-qa-dataset-generator


This project creates a Synthetic Q-A Dataset for RUST language with InContext Learning technique.


## Models Used
2 different models are used for dataset creation; ['gpt3.5'](Gpt_3_5.ipynb) turbo and ['falcon-7b-instruct'](falcon_7b_instruct.ipynb)- I used quantization to load the model in 4 bit.

## Data
Dataset creation was based on the documentation of ['Rust programming language'](data/rust-doc.pdf).

## Approach

### Data pre-processing:

As the task of interest is the generate quality data from the book, we need to split the book into meaningful and coherent parts with the consideration of max token limitation of the model.
*   For achieving this first I extracted chapter titles together with numbers for subsections.

*  Then text of subsections were extracted.

* If text of subsections were too long, (more tokens than the upperbound) it is splitted from a point in which there is a title within a subsection. This way, the document coherency won't be interrupted and generated data will be meaningful and good quality.

* However, if the text is still above the token limit, it is simply splitted in half.

### TASK
#### The task of interest is to build a coherent simple synthetic dataset which can be used for finetuning an code generator LLM. So, we need data as pairs of problem/instruction and code solution.

- After playing with different ways to prompt the model, I find stating explicitly the nature of the problem and answer as most efficient.
- Prompt: " Read the following document about Rust Programming Language and create a 2 Q&A problems where the question is a coding problem and the answer is the code. {document} "
 ## Results
 #### 1. Example outputs of gpt3.5 turbo:
 **Q1: Write code to create a channel and send an integer value of 10 down the channel.**

```rust
use std::sync::mpsc;

fn main() {
    let (tx, rx) = mpsc::channel();
    tx.send(10).unwrap();
}
```

**Q2: Write code to create a channel and receive a string value from the channel, then print it out.**

```rust
use std::sync::mpsc;

fn main() {
    let (tx, rx) = mpsc::channel();
    tx.send(String::from("Hello")).unwrap();

    let received = rx.recv().unwrap();
    println!("Received: {}", received);
}
```
...

    println!("Received: {}, {}", received1, received2)



 #### 2. Example outputs of falcon-7B-instruct:

**Question: How can we modify the existing "Hello, world!" program to take two parameters?**

Answer: We can modify the program by adding more code that allows us to use two inputs (in this case, two strings) and modify the output based on those inputs. Here is an example code snippet to achieve that:

```
fn main(){
  let string1 = "Hello";
  let string2 = "world";
  
  println!("${string1} ${string2}");
  
  // This will now take two string inputs (in this case, two strings) and output "Hello, World".
}
```





