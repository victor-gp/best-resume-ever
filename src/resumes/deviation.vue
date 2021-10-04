// based on some of the templates in <https://standardresume.co/>

<template>
<div class="resume" id="template"><div id="page-container">
    <div id="header-container">
        <div id="header-left">
            <h2 id="position">{{ person.position }}</h2>
            <h1 id="name">{{ person.name.first + ' ' + person.name.last }}</h1>
        </div>
        <div id="header-right">
            <img id="headshot" src="../../resume/id.jpg" alt="Headshot">
        </div>
    </div>
    <div id="contact-info">
        <span id="location" v-if="person.contact.city"><i class="fa fa-map-marker" aria-hidden="true"></i>{{ person.contact.city }}</span>
        <span id="email"><a :href='"mailto:" + person.contact.email'>
            <i class="fa fa-envelope-o" aria-hidden="true"></i>{{ person.contact.email }}</a></span>
        <span id="phone"><i class='fa fa-phone' aria-hidden="true"></i>{{person.contact.phone}}</span>
        <span id="website" v-if="person.contact.website"><a :href='person.contact.website' target="_blank" rel="noopener noreferrer">
            <i class="fa fa-home" aria-hidden="true"></i>{{ person.contact.website }}</a></span>
        <span id="github" v-if="person.contact.github"><a :href='contactLinks.github' target="_blank" rel="noopener noreferrer">
            <i class="fa fa-github" aria-hidden="true"></i>{{ person.contact.github }}</a></span>
        <span id="linkedin" v-if="person.contact.linkedin"><a :href='contactLinks.linkedin' target="_blank" rel="noopener noreferrer">
            <i class="fa fa-linkedin-square" aria-hidden="true"></i>{{ person.contact.linkedin }}</a></span>
    </div>
    <div id="about" v-if="person.about"><p>{{ person.about }}</p></div>
    <section id="experience-section">
        <header><h2>{{ lang.experience }}</h2><hr/></header>
        <div class="experience" v-for="experience in person.experience" :key="experience.company">
            <div class="row-3-period job-info">
                <div class="col job-position"><h3>{{ experience.position }}</h3></div>
                <div class="col job-company"><span>{{ experience.company }}</span></div>
                <div class="col time-period"><span>{{ experience.timeperiod }}</span></div>
            </div>
            <p class="job-description" v-if="experience.description">{{ experience.description }}</p>
            <ul class="job-bullets" v-if="experience.list">
                <li class="job-bullet" v-for="(item, index) in experience.list" :key="index">
                    <span>{{ item }}</span>
                </li>
            </ul>
        </div>
    </section>
    <section id="education-section">
        <header><h2>{{ lang.education }}</h2><hr/></header>
        <div class="education" v-for="education in person.education" :key="education.degree">
            <div class="row-3-period">
                <div class="col edu-degree"><h3>{{ education.degree }}</h3></div>
                <div class="col edu-institution"><span>{{ education.institution || "Lorem Ipsum" }}</span></div>
                <div class="col time-period"><span>{{ education.timeperiod }}</span></div>
            </div>
            <p class="edu-description">{{ education.description }}</p>
        </div>
    </section>
    <section id="skills-section" v-if="person.skills != []">
        <header><h2>{{ lang.skills }}</h2><hr/></header>
        <ul id="skill-list">
            <li class="skill" :id="'skill-' + skill.name.replace(/[ \.]/g, '_')"
                v-for="skill in person.skills" :key="skill.name"><span>{{ skill.name }}</span></li>
        </ul>
        <p id="skills-knowledge">{{ person.knowledge }}</p>
    </section>
</div></div>
</template>

<script>
import Vue from 'vue';
import { getVueOptions } from './options';

const name = 'deviation';
export default Vue.component(name, getVueOptions(name));
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Barlow:wght@400;500;600&display=swap');
</style>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>

@flavor-color: #2a3ffb;
@off-black: #4f5157;

#page-container {
    box-sizing: border-box;
    // can't set document margins in #template
    margin: 55px 80px 30px;
    font-family: 'Barlow', sans-serif;
    font-size: 9.5pt;
    /* font-size: 13px; */
}
h1, h2, p {
    margin: 0;
}
#header-container {
    display: flex;
}
#header-left {
    h2 {
        font-size:23px;
        font-weight: 500;
        color: @off-black;
    }
    h1 {
        font-size: 42px;
        font-weight: 500;
        padding-top: 3px;
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
        position: absolute;
        max-height: 100%;
        object-fit: scale-down;
        right: 0;

        border-radius: 50%; // makes the photo rounded
        /* padding-bottom: 10px; */ // looks eggy but cool!
        box-shadow: 0 0 2px @flavor-color; // adds a subtle border around the photo
        /* image-rendering: pixelated; */ // fix blurry downscaled pictures on Chrome
    }
}
#contact-info {
    margin-top: 10px;
    display: flex;
    justify-content: space-between;
    span {
        // use this if justify-content leaves too much space between elements
        /* margin-right: 20px; */
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
#about > p {
    font-size: 1.05em;
    margin-top: 1.5em;
}
section > header {
    overflow: hidden;
    white-space: nowrap;
    color: @flavor-color;
    h2 {
        display: inline-block;
        font-size: 1.375em;
        font-weight: bold;
        margin-top: 2em;
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
.row-3-period {
    display: flex;
    .col {
        margin-top: auto; // bottom align
    }
    .col:nth-of-type(1) {
        flex: 0 40%; // left align 2nd cols
        h3 {
            display: inline;
        }
    }
    .col:nth-of-type(3).time-period {
        flex-grow: 1;
        position: relative;
        span {
            position: absolute;
            right: 0; // right align
            bottom: 0; // (absolute) bottom align
            font-weight: bold;
            color: @off-black;
        }
    }
}
// first block
header + .experience {
    margin-top: calc(1.25 * 1.5em);
}
.experience {
    // later blocks
    margin-top: calc(1.25 * 2em);
    .job-info {
        margin-bottom: calc(1.25 * 1.5em);
        .job-position > h3 {
            font-size: 1.25em;
            font-weight: bold;
        }
        .job-company {
            font-weight: bold;
        }
    }
    ul.job-bullets {
        padding-left: 0;
        list-style-position: outside;
        margin: 0;
    }
    li.job-bullet {
        margin-left: 1.2em;
    }
    .job-bullet + .job-bullet {
        margin-top: 1ex;
    }
}
// first block
header + .education {
    margin-top: calc(1.15 * 1.5em);
}
.education {
    // later blocks
    margin-top: calc(1.15 * 2em);
    .edu-degree > h3 {
        font-size: 1.15em;
        font-weight: 500;
    }
    .edu-institution > span {
        font-weight: 500;
    }
    .edu-description {
        margin-top: 1em;
    }
}
#skill-list { // <ul>
    display: flex;
    flex-flow: row wrap;
    padding-left: 0; // no indent
    list-style-type: none; // no bullets
    margin: 1.5em 0 0;
    // rows are left aligned and irregular, better avoid any 100% lines
    margin-right: 4%;
    li.skill {
        font-weight: bold;
        padding: 0 1em;
        border-left: 1px solid;
        margin-bottom: 0.8em;
    }
    // patch the leading item of every row
    .skill:nth-of-type(1),
    #skill-Dummy,
        padding-left: 0;
        border-left: none;
    }
    // patch items that may fit in the previous row
    #skill-Dummy_with_spaces_ending_in_dot_ {
        padding-right: 0;
    }
}
#skills-knowledge {
    margin-top: 1.5em;
}
</style>
