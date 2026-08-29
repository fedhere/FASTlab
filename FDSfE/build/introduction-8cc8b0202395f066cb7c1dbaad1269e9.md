# Pedagogy {#ch:pedagogy}

*The following material is currently unpublished, but it is shared
publicly in a dedicated repository[^1]. Our team (myself, Dr. Federica
Bianco, and Dr. Farid Qamar, who co-taught the course in 2024) is in
discussion with publishers ( Princeton Press) to gauge their interest in
turning these teaching materials into an undergraduate level textbook.*

## Introduction {#ch:pedagogy:intro}

### Motivation

The previous chapters of this dissertation have focused on the
development and application of machine learning methods for supernova
classification. This chapter departs from direct astrophysical research
to address a complementary endeavor: teaching data science.

Data science has become an essential skill across all scientific
disciplines
[@hey_fourth_2009; @porcu_data_2026; @vinuesa_decoding_2026; @antonucci_data_2023; @fu_ai_2026; @hersh_beyond_2023; @longo_data_2014; @kang_cultivating_2026; @bednarowska-michaiel_bringing_2026; @zarefard_essential_2024; @dong_are_2026; @gunklach_beyond_2025].
The ability to analyze data, build models, and interpret results is no
longer the exclusive purview of statisticians and computer scientists.
These skills are increasingly expected of researchers and students in
physics and astronomy, and any discipline that deals with data. Students
arrive in the classroom with diverse academic backgrounds
[@bednarowska-michaiel_bringing_2026; @inproceedings], widely varying
levels of mathematical preparation and often only a modicum of
programming experience or less; thus, the question asks itself: "How do
you teach data science?"

### Context

The course "Foundations of Data Science for Everyone" (hereafter FDSE)
was developed under National Science Foundation (NSF) award 2123264
"Collaborative Research: HDR DSC: Delaware and Mid-Atlantic Data Science
Corps" (P.I. Bianco) to expand data science education at Lincoln
University of Pennsylvania and Delaware State University. The materials
presented in this chapter represent the culmination of several years of
development. I first encountered this course as a teaching assistant,
supporting Dr. Federica Bianco on this course for two semesters. I also
served as her teaching assistant for a more advanced course covering
similar material, "Data Science for Physical Scientists." These courses
gave me considerable experience on the 'other side' of the classroom,
which has let me develop my own professional opinions about teaching.
The opportunity arose for me to be the primary instructor for this
course in the Fall of 2025. During my time as professor, I refined and
personalized the materials based on my cumulative experience as a
student, emphasizing accessibility, clarity and conceptual depth.

This chapter presents the pedagogical materials ( written lectures,
assignments and two exams) developed for FDSE. The course was designed
for a broad audience; students from any major were welcome to attend.
The goal was not to produce a professional data scientist in four
months, but rather to equip students with the conceptual knowledge base
and practical skills necessary to engage with data in their own
disciplines and, above all else, the ability to teach themselves.

### Philosophy

The design of this course has several guiding principles, each one
rooted in my experience teaching students and being a student. The first
is that **practice is essential** to understanding. The process of
understanding an idea has several stages, by my own reckoning: ability
to recite the idea upon prompting; ability to recite the idea without
prompting; ability to teach the idea to a peer; ability to use the idea
to create new ideas; ability to teach the idea to a non-peer. Each of
these stages of understanding is effectively 'unlocked' by mastering the
prior stage. In essence: students *must* practice. There is no
substitute for practice.

The second guiding principle is that **students must learn to teach
themselves**. Collective human knowledge is an ever-changing landscape,
and a student's position in that landscape usually starts at the bottom
of a valley --- their sight obscured by hills and mountains, trees and
bushes. Our job as a teacher is to help the student out of one valley so
they might know how to escape the next one on their own.

And at last, a teacher must always ask themselves: **"What does my
student *not* know?"** The most severe barrier to effective teaching I
have seen is an instructor with an unyielding perspective. From moment
to moment or from semester to semester, a teacher must always evaluate
their instruction from the perspective of their student.

While I highlighted these three principles derived from my own
experience as a student and a teacher, there is a wealth of pedagogical
literature that supports their value. For example, there are innumerable
studies done on the importance of practice as a part of learning
[@kang_cultivating_2026; @alzen_training_2024; @dickson_active_2026; @powell_essential_2023; @liu_algebra_2025; @mikula_framework_2017],
as well as the need for students to cooperate and guide their own
learning
[@suhaimee_self-directed_2025; @mercado_self-directed_2024; @balan_evaluation_2025; @forbes-mckay_exploring_2023; @mesghina_cooperative_2024; @silverman_selfdirected_1995].
Self-determined learning, known as heutagogy, is a learning paradigm
that emphasizes the the learner's independence and agency in their
education, and is being actively researched
[@newfield_heutagogy_2025; @panta_heutagogy_nodate]. The "curse of
knowledge" is the phenomenon of experienced teachers failing to
understand what it was once like to not be an expert
[@shatz_curse_2023; @froyd_faculty_2008].

### The theoretical framework of Cognitive Apprenticeship Theory

Teaching Data Science is particularly well-suited to applications of
active learning [@bonwell1991active] and, more specifically, to
Cognitive Apprenticeship Theory [@collins2018cognitive]. Since it deals
with teaching methods for problem-solving in a domain-nonspecific
fashion, it is suited to be taught with direct involvement and intense
use of practical tasks. Given that it is not domain-specific, data can
be chosen to resonate with phenomena relevant to students' lives.

Here, I review the key dimensions and five core teaching methods that
constitute Cognitive Apprenticeship and explain how they are integrated
in the Foundation of Data Science for Everyone course.

For a cognitive apprenticeship to be effective, the learning environment
must be designed around four key dimensions: [@collins2018cognitive].

-   **Content: domain knowledge (facts and concepts), but also the
    "tricks of the trade"---heuristic strategies, control strategies for
    managing one's own thinking, and learning strategies for acquiring
    new knowledge**. All this is taught in FDSE by showing the process
    of data science via live coding. For each topic ( tree methods) the
    instructor reviews the method from a high-level theoretical
    perspective, first conceptually ( "A decision tree is a kind of
    flowchart, where each internal node represents a test --- a question
    --- on a feature," see
    [\[sec:pedagogy:trees\]](#sec:pedagogy:trees){reference-type="ref+label"
    reference="sec:pedagogy:trees"}), then mathematically, obviously
    with very clear step-by-step examples ( "Let's calculate the
    impurity of some of the nodes in
    [1.8](#fig:dtc){reference-type="ref+label" reference="fig:dtc"}.
    Consider the root node, where the total number of samples is
    $N=N_1+N_2=9+5=14$. The relative frequency of 'play' is
    $N_1/N=64\%$, and for 'don't play' it's $N_2/N=36\%$. To calculate
    the Gini impurity:

    $$\begin{aligned}
            G &= 1 - (p_1^2 + p_2^2) \\
            G &= 1 - \left( \left(\frac{N_1}{N}\right)^2 + \left(\frac{N_2}{N}\right)^2 \right) \\
            G &= 1 - \left( 0.64^2 + 0.36^2 \right) \\
            G &= 46\%.
        
    \end{aligned}$$

    Now that we have some practice calculating the Gini impurity on the
    root node, let's do it for each of the three nodes after the root
    node."
    [\[sec:pedagogy:trees\]](#sec:pedagogy:trees){reference-type="ref+label"
    reference="sec:pedagogy:trees"}.) Then, in a live coding session,
    demonstrate the application in real time (see
    [1.1.5](#sec:coursestructure){reference-type="ref+label"
    reference="sec:coursestructure"}). Notebooks are prepared ahead of
    class, of course, but "reconstructed" in front of the student line
    by line so that the student can see the reasoning process, see how
    the instructor "debugs" (their own) mistakes, and how they validate,
    understand and interpret the results the code is showing.

-   **Sequencing: Learning activities should be sequenced to reflect the
    changing demands of learning: increasing in complexity, increasing
    in diversity, and teaching global concepts before local skills. The
    environment should start with simpler, more familiar tasks and
    gradually introduce more complex and diverse problems.** Data
    Science is naturally suited to this process since there is a natural
    sequence in the discipline: tools underpin the whole infrastructure,
    so coding and statistics are addressed first. As part of this,
    students receive a gentle introduction to data with simple datasets
    to demonstrate what was taught about coding and statistics. Then
    they use the tools to start working on data preparation. Meanwhile,
    they are learning about different models and can start on model
    selection, then model implementation. The curriculum outlined below
    follows this paradigm
    ([1.1.5](#sec:coursestructure){reference-type="ref+label"
    reference="sec:coursestructure"}).

-   **Sociology: This dimension emphasizes the social context of
    learning. It involves creating a culture of expert practice where
    students learn through social interaction, cooperation, and by
    becoming part of a "community of practice".** Once again, data
    science as a chiefly collaborative discipline where different skills
    are required naturally provides this. Students in FDSE are
    encouraged to work in groups, where individual skills are expected
    to be elevated: statistics, domain knowledge, coding expertise,
    visualization skills, and writing skills; all and more can be key to
    providing one's contribution. Students are actively encouraged to
    reflect on how they individually contribute to the work: while they
    can turn in assignments as a group, they are required to indicate
    who they worked with and how they have participated in the work.

-   **Methods: This encompasses the five teaching methods described
    below (modeling, coaching, articulation, reflection, and
    exploration).** The use of these methods in FDSE is detailed below,
    grouped into three phases (as described in [@knilt2019modeling]):

<!-- -->

-   *Modeling: An expert (typically the teacher) performs a task while
    explaining their reasoning out loud. This allows students to build a
    conceptual model of the cognitive processes required to accomplish
    the task.* The lectures involve live-coding in real time by the
    instructor (with subsets of the code assigned as in-class exercises,
    see *Coaching* and *Scaffolding*). Code is written in
    `jupyter notebooks`, but the notebooks are developed in real time,
    instead of being pre-compiled and explained in front of the
    students. Each line of code is described, justified, and explained
    as it is being written, and alternative options can be shown.

-   *Coaching: The teacher observes the student as they perform a task,
    and provides hints, feedback, and new tasks as needed. The mentor
    plays an active role in guiding the student's performance.* Lectures
    are deliberately interrupted to give time to students, individually
    or in groups, to complete sections of the notebook that form the
    skeleton of the lecture live-coding portion. These can be tasks that
    review their knowledge from previous lectures ( data preparation
    tasks after Lesson 2), or stretch the students' understanding of the
    current material ("How would you implement a binary choice bases on
    a feature of this dataset, how would you decide which feature to use
    based on the associated Gini impurity? Give it a try in groups!").
    On Zoom, the instructors alternate visiting students' breakout rooms
    (in person, each group would be working in a different area of the
    classroom). The instructors rotate visiting each working group to
    observe how they approach the task, ensure effective collaboration
    practices are in place, and correct the course or offer extension
    tasks as needed.

When successfully implemented, this is analogous to the Think-Pair-Share
methodology [@lyman1981responsive] of demonstrated effectiveness in
physics [@rahman2025impact; @gok2018evaluation] (but also see
[@cooper2021reconsidering]).

-   *Articulation: Students are encouraged to articulate their
    knowledge, reasoning, or problem-solving processes. This can involve
    explaining their thought process to others or a teacher, which helps
    solidify their understanding.* In their assignments, students are
    asked to make plots and explain what the plot shows. At a high
    level, this is the most visible task a (data) scientist performs to
    share knowledge, and knowledge sharing requires introspection and
    articulation. A note is in order here; with recent advances in AI, a
    student could not only ask AI to write their code, but also ask AI
    to describe the resulting figures and results. The topic of teaching
    in the AI era and the pedagogical implications
    [@beale_computer_2025; @taback_generative_2026] are beyond the scope
    of this dissertation, but I am necessarily paying attention to this
    issue and adapting my pedagogy to it.

-   *Reflection: Students are prompted to compare their own
    problem-solving processes with those of an expert, a peer, or an
    internal model of expertise. This helps them identify differences
    and improve their own strategies.* Active coding sessions end with
    the instructor showing the way they solved the problem. Homework
    solutions are assigned for review, particularly ahead of midterm and
    final exams.

<!-- -->

-   *Exploration: The teacher sets general goals for the student and
    encourages them to formulate their own sub-goals and problems to
    solve. This pushes students to work independently and take on the
    role initially held by the mentor.* In more advanced classes, the
    final exam is implemented as a group project, where the students
    begin with ideating, proposing, and then finally performing the
    project. This is not always possible in undergraduate classes,
    however, due to the fast pace and learning requirements.

### Course Structure {#sec:coursestructure}

The course is organized into ten lessons that each cover a broad machine
learning or data science topic. Each lesson is a written guide,
explaining each topic to the appropriate depth that lets the student
imagine how they might apply the concept to an example. The lesson notes
are shared below for each topic with a lecture component (all but the
introduction to Python coding, which is entirely developed as a live
coding session). Each lesson has a companion `Jupyter` notebook (written
in Python) that serves as an in-class live-coding exercise to
immediately demonstrate the applications of the concepts shared in the
lecture. Most lessons also come with an associated homework with clear
objectives and criteria for assessment. These are available at the
dedicated class GitHub repository, along with two exams.[^2] An example
homework and both exams are also included in full in the appendix.

Lesson 1: Python 101

:   Introductory lesson on Python, starting as an absolute beginner and
    covering arithmetic, loops, conditional expressions and functions.

Lesson 2: Data Exploration

:   Exploring data in Python with `Pandas`, data correlation,
    visualizations.

Lesson 3: Statistics

:   Frequentist probability, distributions, the Law of Large Numbers,
    the Central Limit Theorem.

Lesson 4: Null Hypothesis Rejection Testing

:   Null hypothesis framework, the z-test and ks-test.

Lesson 5: Introduction To Machine Learning

:   Data, models, objective functions, data preprocessing,
    hyperparameters and optimization.

Lesson 6: Regression and Classification

:   Linear, multiple linear and logistic regression.

Lesson 7: Tree Models

:   Decision tree methods for classification.

Lesson 8: Clustering

:   $k$-means clustering, dbscan, agglomerative clustering.

Lesson 9: Inferential Neural Networks

:   Introduction to neural networks, dense layers, convolutional neural
    networks.

Lesson 10: Generative Neural Networks

:   Autoencoders for super-resolution, an overview of LLMs.


[^1]: https://github.com/FoxFortino/SaturnineQuail-pedagogy

[^2]: <https://github.com/FoxFortino/SaturnineQuail-pedagogy>