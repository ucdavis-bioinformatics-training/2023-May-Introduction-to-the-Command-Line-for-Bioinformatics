<script>
function buildQuiz(myq, qc){
  // variable to store the HTML output
  const output = [];

  // for each question...
  myq.forEach(
    (currentQuestion, questionNumber) => {

      // variable to store the list of possible answers
      const answers = [];

      // and for each available answer...
      for(letter in currentQuestion.answers){

        // ...add an HTML radio button
        answers.push(
          `<label>
            <input type="radio" name="question${questionNumber}" value="${letter}">
            ${letter} :
            ${currentQuestion.answers[letter]}
          </label><br/>`
        );
      }

      // add this question and its answers to the output
      output.push(
        `<div class="question"> ${currentQuestion.question} </div>
        <div class="answers"> ${answers.join('')} </div><br/>`
      );
    }
  );

  // finally combine our output list into one string of HTML and put it on the page
  qc.innerHTML = output.join('');
}

function showResults(myq, qc, rc){

  // gather answer containers from our quiz
  const answerContainers = qc.querySelectorAll('.answers');

  // keep track of user's answers
  let numCorrect = 0;

  // for each question...
  myq.forEach( (currentQuestion, questionNumber) => {

    // find selected answer
    const answerContainer = answerContainers[questionNumber];
    const selector = `input[name=question${questionNumber}]:checked`;
    const userAnswer = (answerContainer.querySelector(selector) || {}).value;

    // if answer is correct
    if(userAnswer === currentQuestion.correctAnswer){
      // add to the number of correct answers
      numCorrect++;

      // color the answers green
      answerContainers[questionNumber].style.color = 'lightgreen';
    }
    // if answer is wrong or blank
    else{
      // color the answers red
      answerContainers[questionNumber].style.color = 'red';
    }
  });

  // show number of correct answers out of total
  rc.innerHTML = `${numCorrect} out of ${myq.length}`;
}
</script>

# Session 2

## History Repeats Itself

Linux remembers everything you've done (at least in the current shell session), which allows you to pull steps from your history, potentially modify them, and redo them. This can obviously save a lot of time and typing.

The 'head' command views the first 10 (by default) lines of a file. The 'tail' commands views the last 10 (by default) lines of a file. Type 'man head' or 'man tail' to consult their manuals.

    <up arrow>  # last command
    <up>  # next-to-last command
    <down>  # last command, again
    <down>  # current command, empty or otherwise
    history  # usually too much for one screen, so ...
    history | head # we discuss pipes (the vertical bar) below
    history | tail
    history | less # use 'q' to exit less
    ls -l
    pwd
    history | tail
    !560  # re-executes 560th command (yours will have different numbers; choose the one that recreates your really important result!)

## Editing Yourself

Here are some more ways to make editing previous commands, or novel commands that you're building up, easier:

    <up><up>  # go to some previous command, just to have something to work on
    <ctrl-a>  # go to the beginning of the line
    <ctrl-e>  # go to the end of the line
    # left and right arrow keys will move one character at a time
    <ctrl-left> # will move one word at a time to the left
    <ctrl-right> # will move one word at a time to the right
    # now use left and right to move to a single word (surrounded by whitespace: spaces or tabs)
    <ctrl-k>  # delete from here to end of line
    <ctrl-w>  # delete from here to beginning of preceeding word
    blah blah blah<ctrl-w><ctrl-w>  # leaves you with only one 'blah'

You can also search your history from the command line:

    <ctrl-r>fir  # should find most recent command containing 'fir' string: echo 'first' > test.txt
    <enter>  # to run command
    <ctrl-c>  # get out of recursive search
    <ctr-r>  # repeat <ctrl-r> to find successively older string matches


## Create and Destroy

We already learned one command that will create a file, touch. Now let's look at create and removing files and directories.

    cd  # home again
    mkdir ~/tmp2
    cd ~/tmp2
    echo 'Hello, world!' > first.txt

echo text then redirect ('>') to a file.

    cat first.txt  # 'cat' means 'concatenate', or just spit the contents of the file to the screen

why 'concatenate'? try this:

    cat first.txt first.txt first.txt > second.txt
    cat second.txt

OK, let's destroy what we just created:

    cd ../
    rmdir tmp2  # 'rmdir' meands 'remove directory', but this shouldn't work!
    rm tmp2/first.txt
    rm tmp2/second.txt  # clear directory first
    rmdir tmp2  # should succeed now

So, 'mkdir' and 'rmdir' are used to create and destroy (empty) directories. 'rm' to remove files. To create a file can be as simple as using 'echo' and the '>' (redirection) character to put text into a file. Even simpler is the 'touch' command.

    mkdir ~/cli
    cd ~/cli
    touch newFile
    ls -ltra  # look at the time listed for the file you just created
    cat newFile  # it's empty!
    sleep 60  # go grab some coffee
    touch newFile
    ls -ltra  # same time?

So 'touch' creates empty files, or updates the 'last modified' time. Note that the options on the 'ls' command you used here give you a Long listing, of All files, in Reverse Time order (l, a, r, t).

## Forced Removal

When you're on the command line, there's no 'Recycle Bin'. Since we've expanded a whole directory tree, we need to be able to quickly remove a directory without clearing each subdirectory and using 'rmdir'.

    cd
    mkdir -p rmtest/dir1/dir2 # the -p option creates all the directories at once
    rmdir rmtest # gives an error since rmdir can only remove directories that are empty
    rm -rf rmtest # will remove the directory and EVERYTHING in it

Here -r = recursively remove sub-directories, -f means *force*. Obviously, be careful with 'rm -rf', there is no going back, if you delete something with rm, rmdir its gone! **There is no Recycle Bin on the Command-Line!**

## Quiz 3

<div id="quiz3" class="quiz"></div>
<button id="submit3">Submit Quiz</button>
<div id="results3" class="output"></div>
<script>
quizContainer3 = document.getElementById('quiz3');
resultsContainer3 = document.getElementById('results3');
submitButton3 = document.getElementById('submit3');

myQuestions3 = [
  {
    question: "In the command 'rm -rf rmtest', what is 'rmtest'?",
    answers: {
      a: "An option",
      b: "An argument",
      c: "A command",
      d: "A choice"
    },
    correctAnswer: "b"
  },
  {
    question: "Make a directory called test and then run 'rm test'. What happens?",
    answers: {
      a: "Nothing happens",
      b: "The directory is removed",
      c: "The terminal exits",
      d: "You get an error message"
    },
    correctAnswer: "d"
  }
];

buildQuiz(myQuestions3, quizContainer3);
submitButton3.addEventListener('click', function() {showResults(myQuestions3, quizContainer3, resultsContainer3);});
</script>

## Piping and Redirection

Pipes ('\|') allow commands to hand output to other commands, and redirection characters ('>' and '>>') allow you to put output into files.

    echo 'first' > test.txt
    cat test.txt # outputs the contents of the file to the terminal
    echo 'second' > test.txt
    cat test.txt
    echo 'third' >> test.txt
    cat test.txt

The '>' character redirects output of a command that would normally go to the screen instead into a specified file. '>' overwrites the file, '>>' appends to the file.

The 'cut' command pieces of lines from a file line by line. This command cuts characters 1 to 3, from every line, from file 'test.txt'

    cut -c 1-3 test.txt  

same thing, piping output of one command into input of another

    cat test.txt | cut -c 1-3  

This pipes (i.e., sends the output of) cat to cut to sort (-r means reverse order sort), and then grep searches for pattern ('s') matches (i.e. for any line where an 's' appears anywhere on the line.)

    cat test.txt | cut -c 1-3 | sort -r
    cat test.txt | cut -c 1-3 | sort -r | grep s

This is a great way to build up a set of operations while inspecting the output of each step in turn. We'll do more of this in a bit.


## Compression and Archives

As file sizes get large, you'll often see compressed files, or whole compressed folders. Note that **any good bioinformatics software** should be able to work with compressed file formats. 

    gzip test.txt
    cat test.txt.gz

To uncompress a file

    gunzip -c test.txt.gz

The '-c' leaves the original file alone, but dumps expanded output to screen

    gunzip test.txt.gz  # now the file should change back to uncompressed test.txt

Tape archives, or .tar files, are one way to compress entire folders and all contained folders into one file. When they're further compressed they're called 'tarballs'. We can use wget (web get).

    wget -L -O PhiX_Illumina_RTA.tar.gz http://igenomes.illumina.com.s3-website-us-east-1.amazonaws.com/PhiX/Illumina/RTA/PhiX_Illumina_RTA.tar.gz

The .tar.gz and .tgz are *commonly used* extensions for compressed tar files, when gzip compression is used. The application tar is used to uncompress .tar files

    tar -xzvf PhiX_Illumina_RTA.tar.gz

Here -x = extract, -z = use gzip/gunzip, -v = verbose (show each file in archive), -f filename

Note that, unlike Windows, linux does not depend on file extensions to determine file behavior. So you could name a tarball 'fish.puppy' and the extract command above should work just fine. The only thing that should be different is that tab-completion doesn't work within the 'tar' command if it doesn't see the 'correct' file extension.    


## BASH Wildcard Characters

We can use 'wildcard characters' when we want to specify or operate on sets of files all at once.

    ls ?hiX/Illumina

list files in Illumina sub-directory of any directory ending in 'hiX'

    ls PhiX/Illumina/RTA/Sequence/*/*.fa

list all files ending in '.fa' a few directories down. So, '?' fills in for zero or one character, '\*' fills in for zero or more characters. The 'find' command can be used to locate files using a similar form.

    find . -name "*.f*"
    find . -name "*.f?"

how is this different from the previous ls commands?

#### Quick Note About the Quote(s)

The quote characters " and ' are different. In general, single quotes preserve the *literal* meaning of all characters between them. On the other hand, double quotes allow the shell to see what's between them and make substitutions when appropriate. For example:

    VRBL=someText
    echo '$VRBL'
    echo "$VRBL"

However, some commands try to be 'smarter' about this behavior, so it's a little hard to predict what will happen in all cases. It's safest to experiment first when planning a command that depends on quoting ... list filenames first, instead of changing them, etc. Finally, the 'backtick' characters \` (same key - unSHIFTED - as the tilde ~) causes the shell to interpret what's between them as a command, and return the result.

     # counts the number of lines in file and stores result in the LINES variable
    LINES=`cat PhiX/Illumina/RTA/Sequence/Bowtie2Index/genome.1.bt2 | wc -l` 
    echo $LINES


## Symbolic Links

Since copying or even moving large files (like sequence data) around your filesystem may be impractical, we can use links to reference 'distant' files without duplicating the data in the files. Symbolic links are disposable pointers that refer to other files, but behave like the referenced files in commands. I.e., they are essentially 'Shortcuts' (to use a Windows term) to a file or directory.

The 'ln' command creates a link. **You should, by default, always create a symbolic link using the -s option.**

    ln -s PhiX/Illumina/RTA/Sequence/WholeGenomeFasta/genome.fa .
    ls -ltrhaF  # notice the symbolic link pointing at its target
    grep -c ">" genome.fa

## STDOUT & STDERR

Programs can write to two separate output streams, 'standard out' (STDOUT), and 'standard error' (STDERR). The former is generally for direct output of a program, while the latter is supposed to be used for reporting problems. I've seen some bioinformatics tools use STDERR to report summary statistics about the output, but this is probably bad practice. Default behavior in a lot of cases is to dump both STDOUT and STDERR to the screen, unless you specify otherwise. In order to nail down what goes where, and record it for posterity:

    wc -c genome.fa 1> chars.txt 2> any.err

the 1st output, STDOUT, goes to 'chars.txt'  
the 2nd output, STDERR, goes to 'any.err'  

    cat chars.txt

Contains the character count of the file genome.fa

    cat any.err

Empty since no errors occured.

Saving STDOUT is pretty routine (you want your results, yes?), but remember that explicitly saving STDERR is important on a remote server, since you may not directly see the 'screen' when you're running jobs.


## Quiz 4

<div id="quiz4" class="quiz"></div>
<button id="submit4">Submit Quiz</button>
<div id="results4" class="output"></div>
<script>
quizContainer4 = document.getElementById('quiz4');
resultsContainer4 = document.getElementById('results4');
submitButton4 = document.getElementById('submit4');

myQuestions4 = [
  {
    question: "In the PhiX directory, count the number of the files ending in '.fa'. You will need to use the 'wc' command:",
    answers: {
      a: "15",
      b: "9",
      c: "7",
      d: "10"
    },
    correctAnswer: "d"
  },
  {
    question: "Which of the following commands will list all of the txt files in all the 'Genes' directories underneath the 'Archives' directory?",
    answers: {
      a: "ls PhiX/Illumina/RTA/*o*/Archives/*/*/*.txt",
      b: "ls PhiX/Illumina/RTA/A*/Archives/*/*.txt",
      c: "ls PhiX/Illumina/RTA/S*/Archives/*/*/*.txt",
      d: "ls PhiX/Illumina/RTA/Annotation/Genes/*.text"
    },
    correctAnswer: "a"
  },
  {
    question: "Use pipes to get nucleotides 640 to 700 from the sequence in the genome.fa file:",
    answers: {
      a: "GCTCGTCGCTGCGTTGAGGCTTGCGTTTATGGTACGCTGGACTTTGTAGGATACCCTCGCT",
      b: "TATTAAGCTCATTCAGGCTTCTGCCGTTTTGGATTTAACCGAAGATGATTTCGATTTTCTG",
      c: "ATTATGTTCATCCCGTCAACATTCAAACGGCCTGTCTCATCATGGAAGGCGCTGAATTTAC",
      d: "GGATTACTATCTGAGTCCGATGCTGTTCAACCACTAATAGGTAAGAAATCATGAGTCAAGT"
    },
    correctAnswer: "c"
  }
];

buildQuiz(myQuestions4, quizContainer4);
submitButton4.addEventListener('click', function() {showResults(myQuestions4, quizContainer4, resultsContainer4);});
</script>


HOMEWORK
---------

Download the following file:

http://igenomes.illumina.com.s3-website-us-east-1.amazonaws.com/Escherichia_coli_K_12_MG1655/NCBI/2001-10-15/Escherichia_coli_K_12_MG1655_NCBI_2001-10-15.tar.gz

Uncompress and extract the archive. Find all of the files named "refGene.txt". Find the file sizes for each of them. Now find a file called "adapter_contam1.fa". Look at the file. Use pipes to find the length of the adapter.
