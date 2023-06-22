---
layout: post
title: Creating a LaTeX Resume that Stands Out from "Jake's Resume" 
description:
date:   2023-04-10
image:  '/images/latex_template.jpg'
tags:   [LaTeX, Overleaf]
---

If you've ever felt like your resume is a bland, uninspired reflection of your skills and experiences, you're not alone. I, too, was tired of blending in with a sea of generic resumes based on the popular LaTeX resume template called [Jake's Resume](https://www.overleaf.com/latex/templates/jakes-resume/syzfjbzwjncs), which, despite its professional appearance, has become somewhat overused. Unwilling to settle for average, I was determined to create a resume that would grab attention and showcase my individuality. I took a deep dive into the esoteric world of LaTeX, a powerful document preparation system known for its excellent typography and ability to manage complex formatting with ease.

In this blog post, I will hopefully inspire you to create your own standout LaTeX resume, or at least use one of my templates for your next job application. Come along as we dive into the obscure code of LaTeX, explore the benefits of PDFs, and uncover some of the magic that makes a LaTeX resume so striking. Here is what I came up with:

![Image]({{site.baseurl}}/images/latex_example_2.jpg) 

For your convenience, you have two options to follow along:
1. You can clone or download the required materials from the career_developer_template [directory](https://github.com/oresttokovenko/resume_templates/tree/main/career_developer_template) in my Github repository.
2. Alternatively, if you prefer using Overleaf, this template was published in the Overleaf gallery 🥳. You can make a direct copy from [here](https://www.overleaf.com/latex/templates/resume-template-by-orest/zmrmcnwmxdxn). This was made available as of May 2023. 


Once you open the above link, you will see two files. Let's begin with the `.cls` file - this file holds all the function code, allowing us to benefit from the DRY (Don't Repeat Yourself) principle. There is a bit of preamble that sets up the formatting of the document, but after that you will come to the 'Custom Commands' section. By executing these functions in the `.tex` file, we can maintain a clean and organized codebase while also keeping our resume content concise and focused. In LaTeX, commands are preceded by a backslash `\`. While it might seem unusual at first, you will quickly become accustomed to this syntax as you work with LaTeX. Here is an example of a LaTeX command that simply bolds some text (I prefer to use Pascal Case for LaTeX commands):

```latex
%---------------------------------
%        .cls file
%---------------------------------

\newcommand{\BoldText}[1]{
  \textbf{#1}
}

%---------------------------------
%        .tex file
%---------------------------------

\BoldText{This text will be bolded.}
```
<br>
I imagine it doesn't seem so bad so far? Lots of backslashes, which is an interesting choice and difficult to reach with the pinky finger.

Let's explore creating custom commands to further streamline your resume-building process. Custom commands can be defined using the `\newcommand` directive, which makes repetitive tasks a breeze. For example, if you want to create a command for listing your Cloud certifications, simply add the following to your `.cls` file, and call with arguments in your `.tex file`. 

```latex
%---------------------------------
%        .cls file
%---------------------------------

\newcommand{\Certification}[3]{
    \noindent
    \textbf{#1} $|$ {#2}\hfill \textit{#3}\\
    \vspace{-9pt}
}

%---------------------------------
%        .tex file
%---------------------------------

\Certification
    {Certified Developer - Associate} % credential name
    {Amazon Web Services (AWS)} % issuing institution
    {Aug 2016} % date of completion

``` 
<br>

Let's dissect the structure of the custom command `\Certification` and understand how it works when called with appropriate arguments.

In this example, we are defining a command called `\Certification`. The `[3]` immediately following the command name indicates that our custom command takes three arguments.

Inside the curly braces `{}` is the actual definition of the command, which specifies how the command will behave when called with the appropriate arguments. In this case, the command is structured as follows:

* `\noindent`: This command tells LaTeX not to indent the beginning of the line, keeping the content left-aligned.
* `\textbf{#1}`: This applies the `\textbf` directive (which makes the text bold) to the first argument #1, which corresponds to the "Certification Name" when we call the command later.
* `$|$`: This adds a vertical bar `|` as a separator between the first and second arguments.
* `{#2}`: This is the second argument, representing the "Issuing Organization" when the command is called.
* `\hfill`: This command tells LaTeX to fill the available horizontal space, pushing the following content to the right margin of the line.
* `\textit{#3}\\`: This applies the `\textit` directive (which makes the text italic) to the third argument #3, which corresponds to the "Date Issued" when we call the command later. The double backslash `\\` starts a new line.
* `\vspace{-9pt}`: This command adjusts the vertical space between this entry and the next one. In this case, we are decreasing the space by 9 points, making the entries appear closer together.

When you call the command in your `.tex` file, you are essentially passing these three arguments into the `\Certification command`. LaTeX then processes the command according to its definition, resulting in a neatly formatted certification entry with the certification name in bold, followed by the issuing organization and date issued, all properly aligned. The results look something like this:

![Image]({{site.baseurl}}/images/latex_example_1.jpg) 

The beauty of custom LaTeX commands lies in their versatility and adaptability. You can replicate the method of creating custom commands to accommodate different sections of your resume, such as Education, Work Experience, and Skills. By tailoring commands to suit the specific requirements and formatting of each section, you can ensure a uniform and professional appearance throughout your resume. For instance, you could create a custom command for listing your educational background with relevant details like degree, institution, and graduation date. 

Similarly, you could design a command for showcasing your skills with an appropriate format that highlights your proficiency levels. Embracing the power of custom LaTeX commands allows you to not only maintain a clean and consistent structure in your resume but also to craft a truly unique and personalized document that showcases your individuality and achievements. With a few lines of code, you can create neatly organized, visually appealing sections that highlight your achievements.

LaTeX may require more effort upfront, especially for those who are new to the language, but it offers significant advantages when it comes to tailoring each resume to specific job postings. While Word-based resumes often involve time-consuming adjustments to formatting, LaTeX provides a more efficient and streamlined process.

With LaTeX, you can easily customize the content of your resume without worrying about formatting inconsistencies. This is because LaTeX separates the content from the presentation, allowing you to focus on what matters most: showcasing your skills, experiences, and achievements relevant to the job posting. When you make changes to your resume, the LaTeX compiler automatically takes care of the formatting, ensuring a consistent and professional appearance.

If you would like to use my LaTeX resume template as a starting point for your own, or simply use my template, feel free to fork my [GitHub repository](https://github.com/oresttokovenko/resume_templates) where I put together a few LaTeX templates. If you like it, please give it a Star. By harnessing the power of LaTeX, you can create a resume that not only stands out visually but also showcases your skills, experience, and individuality in a clear and professional manner. Give it a try, and you might just find that your days of blending in with the crowd are behind you.

In conclusion, using LaTeX for your resume offers several advantages over other resume-building tools, such as excellent typography, the ability to easily manage complex formatting, and the opportunity to create custom commands that can streamline your resume-building process. With a little time and effort, you can create a truly unique and personalized resume that will help you stand out in the job market. So go ahead, dive into the world of LaTeX, and start crafting a resume that reflects your individuality and showcases your accomplishments.