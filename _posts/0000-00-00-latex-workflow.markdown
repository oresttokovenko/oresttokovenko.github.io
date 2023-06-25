---
layout: post
title:  Automating the Workflow for Job Applications using Bash
date:   2023-06-25
image:  '/images/latex_workflow.jpg'
tags:   [bash, makefile, python, LaTeX]
---

Navigating the job market as a Data Engineer often feels like maneuvering through a complex maze. The maze gets tougher with each passing day as the competition in the industry grows fiercer. Amidst this challenge, there's a relentless requirement to stay relevant and appealing to prospective employers, often necessitating a deluge of applications to various roles. With each application comes the necessity to tailor my resume, matching it with the specific requirements of the job. This is where LaTeX comes to the rescue, and my project, the lifeboat that helps me sail smoothly.

LaTeX makes the task of resume tailoring less daunting, as you don't need to concern yourself with the formatting. However, I was still stuck with the redundant process of copying and pasting bash commands from a document for each new job application, renaming files, wasting time with tedious tasks. It felt like trying to paddle upstream - not very efficient or enjoyable. Since I have a soft spot for optimization, I decided it was high time to automate my way out of the inefficiency and streamline my application process. And thus, this project was born.

With this motivation, I created small a project that would cater to my needs, which you can access [here](https://github.com/oresttokovenko/latex-resume-workflow). The project is an collection of a few bash and python scripts that work in tandem with a Makefile. The idea is simple: Run `make new-resume`, and voila! The scripts do their magic and set up everything you need to create a LaTeX resume. I designed the system to consider if the job or company directory already exists, preventing any data overwrite.

![Gif]({{site.baseurl}}/images/latex_workflow_pt1.gif)

I also encountered an interesting trend – some companies delete their job postings after gathering a sufficient number of candidates. A minor inconvenience until you're called for an interview, and all you've got to refer back to is a "404 Page Not Found" instead of the job description. This led me to incorporate a simple yet effective solution in my automated LaTeX workflow. Each time I decide to apply for a job, I set up my LaTeX files and immediately copy the job description into a text file named job_description.txt. This nifty trick serves as a time capsule, capturing the essence of the job posting and acting as a safety net against disappearing job posts. The job_description.txt file then becomes an invaluable reference point for tailoring my resume and preparing for interviews

![Gif]({{site.baseurl}}/images/latex_workflow_pt2.gif)

Now, we've arrived at the pinnacle of our automated workflow: the creation of your resume. In this guide, I've used the 'Software Engineer' [resume template](https://github.com/oresttokovenko/resume_templates/tree/main/career_developer_template) that I personally created and had published to the Overleaf gallery. Feel free to adopt this template or choose one that better suits your career trajectory and preferences.

If you have a base template that you prefer, you can add it to the automation process by simply adding it to the `_helpers` directory and make a quick modification to the `create_resume_files.sh` script to copy it when you generate the resume files.

Once you're satisfied with the content and layout of your resume, the `make compile` command is there to transcribe your LaTeX code into a polished pdf document. One of the great benefits of this automated system is the ability to iterate quickly: with every execution of `make compile`, your latest changes are integrated and immediately visible in the pdf viewer tab in your IDE.

![Gif]({{site.baseurl}}/images/latex_workflow_pt3.gif)

Lastly, run `make final-pdf` to rename and move the output file to a higher directory. This is so that you know what the final resume document will be, and so that you don't need to manually rename it. Additionally, for those tedious links which we all dread filling out for every job application? Use `make links` to generate your LinkedIn and portfolio URLs straight in the command line for easy access.

![Gif]({{site.baseurl}}/images/latex_workflow_pt4.gif)

One of the challenges I encountered was the inability to use the `cd` command inside a bash script. Once the relevant files were created, I wanted the shell to switch to that directory so they could access the second Makefile and compile the resume. It turns out, when you `cd` within a script, the change only exists for the duration of the script. Once the script ends, you're back in the original working directory - rendering the command ineffective! As a workaround, I simply copied the change directory command to the user's clipboard and prompted them to paste it into the command line. There were other, more complex solutions, but I didn't feel that they were worth getting into for this project. I also learned that bash doesn't have Booleans, and I'm not sure what to make of that.

In the end, this project is not just about looking for a new job or creating a resume. It's about optimization. It's about using our skills to automate the repetitive and focus on what truly matters - creating value. After all, why should you waste time reinventing the wheel when you can use that time to invent a rocket? So, here's to more efficiency, more job applications, and of course, more LaTeX!