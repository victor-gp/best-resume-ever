// based on some of the templates in <https://standardresume.co/>

<template>
<div class="resume" id="template"><div id="page-container">
    <div id="header-container">
        <div id="header-left">
            <h2 id="position">{{person.position}}</h2>
            <h1 id="name">{{person.name.first + ' ' + person.name.last}}</h1>
        </div>
        <div id="header-right">
            <img id="headshot" src="../../resume/id.jpg" alt="Headshot">
        </div>
    </div>
    <div id="contact-info">
        <span v-if="person.contact.city" id="location"><i class="fa fa-map-marker" aria-hidden="true"></i>{{person.contact.city}}</span>
        <span id="email"><a :href='"mailto:" + person.contact.email'>
            <i class="fa fa-envelope-o" aria-hidden="true"></i>{{person.contact.email}}</a></span>
        <span id="phone"><i class='fa fa-phone' aria-hidden="true"></i>{{person.contact.phone}}</span>
        <span v-if="person.contact.website" id="website"><a :href='person.contact.website' target="_blank" rel="noopener noreferrer">
            <i class="fa fa-home" aria-hidden="true"></i>{{person.contact.website}}</a></span>
        <span v-if="person.contact.github" id="github"><a :href='contactLinks.github' target="_blank" rel="noopener noreferrer">
            <i class="fa fa-github" aria-hidden="true"></i>{{person.contact.github}}</a></span>
        <span v-if="person.contact.linkedin" id="linkedin"><a :href='contactLinks.linkedin' target="_blank" rel="noopener noreferrer">
            <i class="fa fa-linkedin-square" aria-hidden="true"></i>{{person.contact.linkedin}}</a></span>
    </div>
    <div v-if="person.about" id="about">
        <p>{{person.about}}</p>
    </div>
    <div id="resume-body">
        <div id="experience-container">
            <div class="section-separator"><h2>{{ lang.experience }}</h2><hr/></div>
            <div class="experience" v-for="experience in person.experience" :key="experience.company">
                <div class="row-3 job-info">
                    <div class="col job-company"><h3>{{experience.company}}</h3></div>
                    <div class="col job-position"><span>{{experience.position}}</span></div>
                    <div class="col job-period time-period"><span>{{experience.timeperiod}}</span></div>
                </div>
                <p class="job-description" v-if="experience.description">{{experience.description}}</p>
                <ul v-if="experience.list">
                    <li v-for="(item, index) in experience.list" :key="index">
                      <span class="list-item-black">
                        {{item}}
                      </span>
                    </li>
                </ul>
            </div>
        </div>
        <div id="education-container">
            <div class="section-separator">{{ lang.education }}<hr/></div>
            <div class="education" v-for="education in person.education" :key="education.degree">
                <h2 class="education-description">{{education.description}}</h2>
                <p><span class="degree">{{education.degree}} | </span><span class="education-timeperiod">{{education.timeperiod}}</span></p>
            </div>
        </div>
        <div id="skills-container" v-if="person.skills != []">
            <div class="section-separator">{{ lang.skills }}<hr/></div>
            <p id="skill-description">{{person.knowledge}}</p>
            <ul id="skill-list">
                <li class="skill" v-for="skill in person.skills" :key="skill.name">
                  <span class="list-item-black">
                    {{skill.name}}
                  </span>
                </li>
            </ul>
        </div>
    </div>
</div></div>
</template>

<script>
import Vue from 'vue';
import { getVueOptions } from './options';

const name = 'deviation';
export default Vue.component(name, getVueOptions(name));
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Barlow:wght@400;500&display=swap');
</style>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>

@flavor-color: #2a3ffb;

#page-container {
    box-sizing: border-box;
    // can't set document margins in #template
    margin: 55px 80px 30px;
    font-family: 'Barlow', sans-serif;
    font-size: 12px;

    h1, h2, p {
        margin: 0;
    }
}
#header-container {
    display: flex;
}
#header-left {
    h1 {
        font-size: 42px;
        padding-top: 3px;
    }
    h2 {
        font-size:22px;
    }
}
#header-right {
    position: relative;
    // takes the leftover horizontal space so that #headshot can be right aligned
    flex-grow: 1;
    /* aligns with the bottom of #header-left, cause there's too much negative space below letters.
       (!) this is totally font and font-size dependent, fine tune as needed. */
    margin-bottom: 2px;

    #headshot {
        // along with the parent's relative pos, makes this.height relative to #header-left. somehow XD
        position: absolute;
        max-height: 100%;
        object-fit: scale-down;
        right: 0px;

        border-radius: 50%; // makes the photo rounded
        /* padding-bottom: 10px; // looks eggy but cool! */
        box-shadow: 0 0 2px @flavor-color; // adds a subtle border around the photo
    }
}
#contact-info {
    margin-top: 10px;
    display: flex;
    justify-content: space-between; // takes the full width of the document
    font-size: 13px;

    span {
        // use this if justify-content leaves too much space between elements
        // margin-right: 20px;
    }
    a {
        text-decoration: none; // no underline for links
        color: inherit; // don't change color on :visited
    }
    i {
        margin-right: 0.5em; // space after each icon
        color: @flavor-color;
    }
    .fa-github {
        color: black;
        font-size: 115% // coz it's too small, can't make out the octocat!
    }
    .fa-linkedin-square {
        color: #0077b5; // linkedin blue
    }
}
#about {
    margin-top: 16px;
    font-size: 14px;
}

#resume-body {
    div.section-separator {
        overflow: hidden;
        white-space: nowrap;
        margin-top: 16px;
        font-size: 16px;
        font-weight: bold;
        color: @flavor-color;
        h2 {
            display: inline-block;
            font-size: 1em;
        }
        hr {
            display: inline-block;
            width: 100%;
            position: relative;
            margin-left: 0.8em;
            top: 0.55ex;
            border: none;
            border-bottom: 1pt solid;
            color: inherit;
        }
    }
    .row-3 {
        display: flex;
        .col {
            margin-top: auto; // bottom align
        }
        .col:nth-of-type(1) {
            flex: 0 33%; // left align 2nd cols
        }
        .col:nth-of-type(3) {
            flex: 1;
            position: relative;
            span {
                position: absolute;
                right: 0;
                bottom: 0; // patch bottom align
            }
        }
        h3, span {
            display: inline;
        }
    }

    .experience + .experience {
        margin-top: 2em;
    }
    .experience {
        margin-top: 1em; // applies to first experience only
        .job-info {
            margin-bottom: 1em;
            .job-company h3 {
                font-size: 15px;
                font-weight: bold;
            }
            .job-position {
                font-size: 13px;
                font-weight: bold;
            }
            .job-period {
                font-size: 13px;
                font-weight: bold;
                color: #4f5157;
            }
        }
        ul {
            margin-top: 5px;
        }
    }

    .company, .education-description {
        font-size:20px;
    }

    .job-info {
        margin-bottom:5px;
    }



    .job-title, .degree {
        font-weight:700;
        font-size:16px;
    }

    .experience-timeperiod, .education-timeperiod {
        font-weight:100;
        font-size:16px;
    }

    .education {
        margin: 10px 0 10px;
    }

    #skill-list {
        column-count: 3;
        list-style-position: inside;
        ul li {
            font-size:14px;
        }
    }

    #education-container, #skills-container {
        margin-top: 20px;
    }
}
</style>
