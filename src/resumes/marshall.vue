// based on some of the templates in <https://standardresume.co/>

<template>
<div class="resume" id="template"><div id="page-container">
    <div id="header-container" :class=" noPhoto ? 'no-photo' : 'with-photo' ">
        <div id="header-top">
            <div class="left">
                <h1 id="name">{{ person.name.first + ' ' + person.name.last }}</h1>
                <h2 id="position">{{ person.position }}</h2>
            </div>
            <div class="right">
                <img id="photo" src="../../resume/id.jpg"
                    :alt="`Photo of ${person.name.first + ' ' + person.name.last}`">
            </div>
        </div>
        <div id="contact-info">
            <span id="location" v-if="person.contact.city">
                <i class="fa fa-map-marker icon-left" aria-hidden="true"></i>{{ person.contact.city }}<i class="fa fa-map-marker icon-right" aria-hidden="true"></i>
            </span>
            <span id="email"><a :href='"mailto:" + person.contact.email'>
                <i class="fa fa-envelope-o icon-left" aria-hidden="true"></i>{{ person.contact.email }}<i class="fa fa-envelope-o icon-right" aria-hidden="true"></i></a></span>
            <span id="phone"><i class="fa fa-phone icon-left" aria-hidden="true"></i>{{person.contact.phone}}<i class="fa fa-phone icon-right" aria-hidden="true"></i></span>
            <span id="website" v-if="person.contact.website"><a :href='person.contact.website' target="_blank" rel="noopener noreferrer">
                <i class="fa fa-home icon-left" aria-hidden="true"></i>{{ person.contact.website }}<i class="fa fa-home icon-right" aria-hidden="true"></i></a></span>
            <span id="github" v-if="person.contact.github"><a :href='contactLinks.github' target="_blank" rel="noopener noreferrer">
                <i class="fa fa-github icon-left" aria-hidden="true"></i>{{ person.contact.github }}<i class="fa fa-github icon-right" aria-hidden="true"></i></a></span>
            <span id="linkedin" v-if="person.contact.linkedin"><a :href='contactLinks.linkedin' target="_blank" rel="noopener noreferrer">
                <i class="fa fa-linkedin icon-left" aria-hidden="true"></i>{{ person.contact.linkedin }}<i class="fa fa-linkedin icon-right" aria-hidden="true"></i></a></span>
        </div>
        <div id="about" v-if="person.about" v-html="person.about"></div>
    </div>
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
            <p class="edu-description" v-if="education.description">{{ education.description }}</p>
        </div>
    </section>
    <section id="skills-section" v-if="person.skills != []">
        <header><h2>{{ lang.skills }}</h2><hr/></header>
        <ul id="skill-list">
            <li class="skill" :id="'skill-' + skill.name.replace(/[ \.]/g, '_')"
                v-for="skill in person.skills" :key="skill.name"><span>{{ skill.name }}</span></li>
        </ul>
        <p id="skills-knowledge" v-if="person.knowledge" >{{ person.knowledge }}</p>
    </section>
</div></div>
</template>

<script>
import Vue from 'vue';
import { getVueOptions } from './options';

const name = 'marshall';
const opts = { ...getVueOptions(name) };
opts.props = ['noPhoto'];

/* useful for debugging */
opts.created = function () {
    console.log(this.noPhoto);
};

export default Vue.component(name, opts);
</script>

<style>
@import url('https://fonts.googleapis.com/css2?family=Barlow:wght@400;500;600&display=swap');
</style>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less" scoped>

@accent-color: #fa5728;
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

#name {
    font-size: 42px;
    font-weight: 500;
}
#position {
    font-size:23px;
    font-weight: 500;
    color: @off-black;
    padding-top: 8px;
}

#header-container.with-photo {
    #header-top {
        display: flex;

        .right {
            position: relative;
            // takes the leftover horizontal space so that #photo can be right aligned
            flex-grow: 1;
            // improve alignment with the bottom of #header-left, fine tune as required
            margin-bottom: 4px;
            #photo {
                position: absolute;
                max-height: 100%;
                object-fit: scale-down;
                right: 0;

                // makes the photo rounded
                border-radius: 50%;
                // looks eggy but cool!
                /* padding-bottom: 10px; */
                // adds a subtle border around the photo
                box-shadow: 0 0 2px @accent-color;
                // fix blurry downscaled pictures on Chrome
                /* image-rendering: pixelated; */
            }
        }
    }

    #contact-info {
        margin-top: 10px;
        display: flex;
        justify-content: space-between;

        i.icon-left {
            // space after each icon
            margin-right: 0.5em;
        }

        i.icon-right {
            display: none;
        }

        span {
            // use this if justify-content leaves too much space between elements
            /* margin-right: 20px; */
        }
    }
}

#header-container.no-photo {
    display: flex;
    flex-wrap: wrap;

    #photo {
        display: none;
    }

    #contact-info {
        flex-grow: 1;
        align-items: flex-end;

        display: flex;
        flex-direction: column;
        justify-content: center;

        span + span {
            margin-top: 0.2em;
        }

        i.icon-left {
            display: none;
        }

        i.icon-right {
            width: 1.5rem;
            text-align: center;
            margin-left: .2em;
        }
    }
}

#contact-info {
    a {
        text-decoration: none; // no underline for links
        color: inherit; // don't change color on :visited
    }
    i {
        color: darken(@accent-color, 5%);
    }
    .fa-github {
        color: black;
        font-size: 115%; // coz it's too small, can't make out the octocat!
    }
    .fa-linkedin {
        color: #0077b5; // linkedin blue
    }
}

#about {
    font-size: 1.05em;
    margin: 1.5em 1em 0;
    ::v-deep p {
        margin: 0;
    }
    ::v-deep p + p {
        margin: 0.7em 0 0;
    }
}

#header-container.no-photo > #about {
    margin-top: 2em;
}

section > header {
    overflow: hidden;
    white-space: nowrap;
    color: @accent-color;
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
        margin-left: 1.25em;
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
        margin-bottom: 0.8em;
        position: relative;
    }
    li.skill::before {
        border-left: thin solid black;
        margin: 1px 0 1px;
        -webkit-print-color-adjust: exact;
        -webkit-filter: opacity(1);
        position: absolute;
        top: 0;
        left: 0;
        right: 0;
        bottom: 0;
        content: "";
    }
    // patch the leading item of every row
    .skill:nth-of-type(1),
    #skill-Dummy {
        padding-left: 0;
        &::before {
            border-left: none;
        }
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
