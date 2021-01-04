---
title: 'Dr. Jekyll or: How I Learned to Stop Worrying and Made an Academic Webpage'
date: 2021-01-04
permalink: /posts/2021/01/making-an-academic-webpage/
classes: wide
tags: 
    Jekyll 
    tutorial
---
{% include box.html %}

## Winter project

As a winter vacation project, I decided to build my academic webpage. I am rather new to web development with minimum knowledge of the most common web develpment markups/languages like HTML, JavaScript, etc. Initially, I thought it might be a good idea to build the webpage from scratch using HTML, since I believed it gave me maximum control over the style and build. I even ran through a quick HTML refresher tutorial at the wonderfully designed portal [W3Schools](https://www.w3schools.com/){:target="_blank"}. However, I realized soon that this won't be enough to create a decent-enough modern webpage. A short Google-session for ```building an academic website``` brought me to this [awesome tutorial](https://jayrobwilliams.com/posts/2020/06/academic-website/){:target="_blank"} by Jay Rob Williams. It introduced me to the idea of static-site generators like  **Jekyll** and **Hugo**. Jekyll is written using Ruby while Hugo is coded using Golang. Both have an active user community and offer a great variety of templates to build the site with.

For me, Jekyll seemed like an ideal place for a first academic website for a beginner since

- it has built-in support for GitHub Pages (Tom-Preston Werner, the co-founder of GitHub also developed Jekyll),

- simple markdown is enough to generate content; Jekyll generates the necessary HTML files from that.

Unlike Hugo, Jekyll is not supported natively in Windows. But, since I had switched recently to Ubuntu, it was not an issue. [Minimal Mistakes](https://mmistakes.github.io/minimal-mistakes/){:target="_blank"} is, by far, the most common Jekyll theme, atleast for academics. Stuart Geiger forked Minimal Mistakes to add some academia-specific enhancements (Publications tab, CV tab, etc.) resulting in the amazing [academicpages](https://academicpages.github.io/){:target="_blank"} theme. I am using the [academictemplate](https://github.com/matthewkirby/academictemplate){:target="_blank"} by Matthew Kirby. It has essentially all the features of the academicpages, and some additional ones like out-of-the-box MathJax support, etc. Let me also mention [minima-reboot](https://aterenin.github.io/minima-reboot/){:target="_blank"}, a neat minimal theme for Jekyll, more suited for blogging. Once you decide on a theme, setting up the webpage is mainly about adding content. The overall settings for how the website looks is decided by the ```_config.yml``` file and is rather straightforward. There were a few things concerning the website's appearance for which I had to hunt around a bit. I collect these things below, in case it might be of help to someone.

## Finicky adjustments


Here are a list of not-so-easy-to-find solutions to common problems I encountered while creating the webpage.

1. **Justify content:** 
    I prefer the content throughout the website to be justified. However, this was not available out-of-the-box in the acadmicepages theme. After some searches, I figured out that this can be achieved by adding the following snippet to the file ```_layouts/default.html``` after ```</head>```:

    ```html
        <!--added to get text justification throughout-->
        <style>
        div {
        text-align: justify;
        text-justify: inter-word;
        }
        </style> 
    ```


2. **Adjust global font size:** 
    The overall font sizes for the theme can be found in the ```_sass/minimal-mistakes/_cariable.sass```. However, it is [not advisable to mess around](https://github.com/mmistakes/minimal-mistakes/discussions/1219){:target="_blank"} with the _sass files. The workaround is to add the following snippet to the file ```assets/css/main.scss``` after the line with ```@import "minimal-mistakes"```:
    ```scss
    html {
        font-size: 16px; // change to whatever

        @include breakpoint($medium) {
        font-size: 18px; // change to whatever
        }

        @include breakpoint($large) {
        font-size: 20px; // change to whatever
        }

        @include breakpoint($x-large) {
        font-size: 22px; // change to whatever
        }
    }
    ```


3. **Open sidebar links in new tab:**
    I did not quite like how the default action upon clicking the sidebar icons under the author profile in the homepage (Twitter, GitHub, Scholar, etc.) was to open the new webpage in the same tab. Personally, it is a no-brainer that internal links should open in the same tab and links to external pages should open in a new tab, however I have come across [strong counter arguments](https://www.tempertemper.net/blog/opening-links-in-a-new-tab-or-window-is-better-avoided){:target="_blank"} for this as well. Eitherways, if you want your links to open in new tabs, then you need to alter the file ```_includes/author-profile.html```. Add ```target="_blank" rel="noopener"``` inside the HTML link tag for each of the icons. For example,

    ```html
    {% raw %}
    {% if author.researchgate %}
        <li>
            <a href="https://www.researchgate.net/profile/{{ author.researchgate }}" 
               target="_blank" rel="noopener">
               <i class="ai ai-researchgate" aria-hidden="true"></i> 
               ResearchGate
            </a>
        </li>
    {% endif %}
    {% endraw %}
    ```


4. **Toggle button to reveal BibTex:**
    I wanted nice buttons to show up below my publications for the links to the URL of the journal, the DOI. In addition, I always felt it will be nice to have a button upon clicking which the BiBTeX key for that particular paper will be shown within a box. This is very useful if somebody is looking to cite your work. For this, I used the Javascript code snippet for a toggle-to-open box created by [Jordi Pont-Tuset](https://github.com/jponttuset/jponttuset.github.io){:target="_blank"}. 

    ```html
    <style>
        /*************************************
        The box that contain BibTeX code
        *************************************/
        div.noshow { display: none; }
        div.bibtex {
            margin-right: 0%;
            margin-top: 1em;
            margin-bottom: 1em;
            border: 1px solid silver;
            padding: 0em 0.5em;
        }
        div.bibtex pre { font-size: 75%; overflow: auto;  width: 150%; padding: 0em 0em;}
    </style>
    <script type="text/javascript">
        // Toggle Display of BibTeX
        function toggleBibtex(articleid) {
            var bib = document.getElementById('bib_'+articleid);
            if (bib) {
                if(bib.className.indexOf('bibtex') != -1) {
                    bib.className.indexOf('noshow') == -1?bib.className = 'bibtex noshow':bib.className = 'bibtex';
                }
            } else {
                return;
            }
        }
    </script>    
    ```
    Save the above snippet as a ```.html``` file inside the ```_includes``` folder and add the line
    ```{% raw %} {% include box.html %} {% endraw %}``` at the beginning of ```publications.md``` to access the script where needed. The invocation is based on the bibkey. For example,

    ```html
        <a class="btn--research" href="javascript:toggleBibtex('morCheFdetal20')">BibTeX</a>
        <div id="bib_morCheFdetal20" class="bibtex noshow">
        <pre>
          @InCollection{morCheFdetal20,
          author    = {Chellappa, S. and Feng, L. and de la Rubia, V. and Benner, P.},
          title     = {Adaptive Interpolatory {MOR} by Learning the Error Estimator
                      in the Parameter Domain},
          booktitle = {Model Reduction of Complex Dynamical Systems},
          series    = {International Series of Numerical Mathematics},
          editors   = {Benner, P. and Breiten, T. and Fa{\ss}bender, H. and Hinze, M.
                      and Stykel, T. and Zimmermann, R.},
          publisher = {Springer},
          year      = {2020}
          }
        </pre>
        </div>    
    ```

    I have defined a custom button with ```btn--research``` found in ```_sass/minimal-mistakes/_buttons.scss```. Doing this, I get a button like this:<br>
    <a class="btn--research" href="javascript:toggleBibtex('morCheFdetal20')">BibTeX</a>
    <div id="bib_morCheFdetal20" class="bibtex noshow">
    <pre>
          @InCollection{morCheFdetal20,
          author    = {Chellappa, S. and Feng, L. and de la Rubia, V. and Benner, P.},
          title     = {Adaptive Interpolatory {MOR} by Learning the Error Estimator
                      in the Parameter Domain},
          booktitle = {Model Reduction of Complex Dynamical Systems},
          series    = {International Series of Numerical Mathematics},
          editors   = {Benner, P. and Breiten, T. and Fa{\ss}bender, H. and Hinze, M.
                      and Stykel, T. and Zimmermann, R.},
          publisher = {Springer},
          year      = {2020}
          }
    </pre>
    </div>    



5. **Change shape of author profile image:**
    The default shape of the author profile picture is a circle. If you, like me, do not prefer this, then this can be adjusted. In ```_sass/minimal-mistales/_sidebar.scss``` locate the ```img``` block. There, a ```block-radius``` of 0% is a square while 100% makes it a circle.

    ```scss
    img {
        max-width: 250px;
        border-radius: 0%;

        @include breakpoint($large) {
        padding: 0.5px;
        border: 0.2px solid $border-color;
        }
    }    
    ```


## Looking forward

To me, it is quite amazing that with just a rudimentary knowledge of coding, a decently-functional personalized webpage can be setup with the help of tools like Jekyll. I only wish I had known about this earlier. Hope to explore more of this amazing tool in the days ahead.