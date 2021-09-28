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
            <div class="section-separator">{{ lang.experience }}<hr/></div>
            <div class="experience" v-for="experience in person.experience" :key="experience.company">
                <h2 class="company">{{experience.company}}</h2>
                <p class="job-info"><span class="job-title">{{experience.position}} | </span><span class="experience-timeperiod">{{experience.timeperiod}}</span></p>
                <p class="job-description" v-if="experience.description">{{experience.description}}</p>
                <ul v-if="experience.list" >
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
    box-sizing:border-box;
    margin: 55px 80px 8px; // cannot set border-box & margins in #template
    font-family:'Barlow', sans-serif;

    h1, h2 {
        /*font-family:'Open Sans Condensed', sans-serif;*/
        margin:0;
    }

    p {
        margin:0;
        font-size:12px;
    }

    ul li {
        font-size:12px;
    }
}

#header-container {
    display: flex;
}
#header-left {
    h1 {
        font-size:42px;
        line-height:56px;
        /* todo: the disconnect b/w this & font-size looks good, but it breaks bottom-alignment with the photo,
        makes that look too close to contact-info. I should reproduce it with margins + padding */
    }
    h2 {
        font-size:22px;
    }
}
#header-right {
    position: relative;
    // takes the leftover horizontal space so that #headshot can be right aligned
    flex-grow: 1;

    #headshot {
        // along with the parent's relative pos, makes this.height relative to #header-left. somehow XD
        position: absolute;
        max-height: 100%;
        object-fit: scale-down;
        right: 0px;
        border-radius: 50%;
        // padding-bottom: 10px; // looks eggy but cool!
    }
}

#contact-info {
    margin-top: 8px;
    display: flex;
    justify-content: space-between; // takes the full width of the document

    font-size: 13px;
    color: @flavor-color; // todo: give this some thought

    span {
        //margin-right: 20px;
    }
    i {
        margin-right: 0.5em;
    }
    a {
        text-decoration: none; // no underline for links
        color: inherit; // don't change color on :visited
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
    clear: both;
    margin-top: 16px;
    margin-bottom: 30px;
    p {
        font-size: 14px;
    }
}

#resume-body {
    margin: 10px 0;

    div.section-separator {
        overflow: hidden;
        white-space: nowrap;
        color: @flavor-color;
        font-weight: 500;
        font-size: 16px;

        hr {
            display: inline-block;
            width: 100%;
            position: relative;
            top: 0.55ex;
            margin-left: 0.8em;
            border: none;
            border-bottom: 1pt solid;
            color: @flavor-color;
        }
    }

    .experience {
        margin: 10px 0 10px;
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
