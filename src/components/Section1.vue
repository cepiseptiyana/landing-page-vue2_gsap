<template>
  <section class="container section-1">
    <div class="row">
      <!-- COLUMN 1 -->
      <div class="column">
        <div class="wrapper-nav">
          <div>
            <p>plan</p>
            <div class="under-line"></div>
          </div>
          <div>
            <p>design</p>
            <div class="under-line"></div>
          </div>
          <div>
            <p>build</p>
            <div class="under-line"></div>
          </div>
        </div>

        <div class="container-list">
          <!-- item-1 -->
          <div
            v-for="(data, index) in data_section"
            :key="index"
            class="list-item"
          >
            <img class="icon" :src="data.icon" alt="" />
            <h2>{{ data.heading }}</h2>
            <p>
              {{ data.paragraph }}
            </p>
            <div class="mobile-img is-active">
              <img class="table" :src="data.image" alt="" />
            </div>
          </div>
        </div>
      </div>

      <!-- COLUMN 2 -->
      <!--  -->
      <div class="column">
        <div class="wrapper-image">
          <div
            v-for="(data, index) in data_section"
            :key="index"
            class="list-item is-active"
          >
            <img class="icon" :src="data.icon" alt="" />
            <img class="table img-photo" :src="data.image" alt="" />
          </div>
        </div>
        <p><span>1</span> / <span>3</span></p>
      </div>
    </div>
  </section>
</template>

<script>
import { gsap } from "gsap";
let mm = gsap.matchMedia();
import ScrollTrigger from "gsap/ScrollTrigger";

gsap.registerPlugin(ScrollTrigger);

import Img from "@/assets/images/table_xl.jpg";
import Wall from "@/assets/images/wall_xl.jpg";
import Men from "@/assets/images/men_xl.jpg";

import hamburger from "@/assets/images/hamburger.svg";
import globe from "@/assets/images/globe.svg";
import tag from "@/assets/images/tag.svg";

export default {
  name: "section-1",
  props: {
    msg: String,
  },
  data() {
    return {
      data_section: [
        {
          image: Img,
          icon: hamburger,
          heading: "The sitemap of the experience",
          paragraph:
            "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum. Stet clita kasd gubergren, no sea",
        },
        {
          image: Wall,
          icon: globe,
          heading: "Time to paint the room walls",
          paragraph:
            "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et justo duo dolores et ea rebum.",
        },
        {
          image: Men,
          icon: tag,
          heading: "Magic happens to build it out",
          paragraph:
            "Lorem ipsum dolor sit amet, consetetur sadipscing elitr, sed diam nonumy eirmod tempor invidunt ut labore et dolore magna aliquyam erat, sed diam voluptua. At vero eos et accusam et.",
        },
      ],
    };
  },
  mounted() {
    const section = document.querySelector(".section-1");

    const el_underline = section.querySelectorAll(
      ".column:nth-child(1) .wrapper-nav .under-line"
    );

    const el_items = section.querySelectorAll(
      ".column:nth-child(1) .container-list .list-item"
    );

    const el_wraper_photo = section.querySelectorAll(
      ".column:nth-child(2) .wrapper-image .list-item"
    );

    const el_photo = section.querySelectorAll(
      ".column:nth-child(2) .wrapper-image .list-item .img-photo"
    );

    const el_icons = section.querySelectorAll(
      ".column:nth-child(2) .wrapper-image .list-item .icon"
    );

    const number = section.querySelector(
      ".column:nth-child(2) p span:first-child"
    );

    const data_list = this.data_section;

    mm.add("(min-width: 900px)", () => {
      ScrollTrigger.create({
        trigger: section,
        start: "top top",
        end: `+=${data_list.length * 100}%`,
        scrub: true,
        pin: true,
        onUpdate: (self) => {
          const total = data_list.length;
          const rawIndex = self.progress * total;
          const index = Math.min(Math.floor(rawIndex), total - 1);
          const localProgress = rawIndex - index;

          // TRIGGER UNDERLINE NAVBAR
          el_underline.forEach((el, i) => {
            if (i === index) {
              el.style.width = `${localProgress * 100}%`;
              el.classList.add("active");
            } else {
              el.style.width = "0%";
              el.classList.remove("active");
            }
          });

          // TRIGGER DESCRIPTION
          el_items.forEach((e, i) => {
            e.classList.toggle("is-active", i === index);
          });

          // TRIGGER PHOTO
          el_wraper_photo.forEach((e, i) => {
            e.classList.toggle("is-active", i === index);
          });

          el_photo.forEach((e, i) => {
            if (index == i) {
              const start = index / total;
              const end = (index + 1) / total;

              let p = (self.progress - start) / (end - start);
              p = Math.max(0, Math.min(1, p));

              const scale = 1.3 - 0.3 * p;

              e.style.transform = `scale(${scale})`;
            }
          });

          el_icons.forEach((e, i) => {
            if (index == i) {
              const start = i / total;
              const end = (i + 1) / total;

              let p = (self.progress - start) / (end - start);

              const scale = 1 + 0.3 * p;

              e.style.transform = `translate(-50%, -50%) scale(${scale})`;
            }
          });

          const jumlah_halaman = data_list.length;
          //   // TRIGGER JUMLAH HALAMAN
          for (let i = 0; i <= jumlah_halaman; i++) {
            if (index == i) {
              number.innerHTML = i + 1;
            }
          }
        },
      });
    });
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/styles/variables";

.container.section-1 {
  background-color: $dark-color;
  height: 100vh;

  & .row {
    height: 100%;
    box-sizing: border-box;
    display: flex;

    & .column {
      flex: 1;
      position: relative;

      display: flex;
      flex-direction: column;
      justify-content: space-between;
      align-items: stretch;
      overflow: hidden;
      box-sizing: border-box;
    }

    & .column:nth-child(1) {
      padding: 3em;

      & .wrapper-nav {
        display: flex;
        gap: 0.88em;

        & div {
          & p {
            font-size: 2.94em;
            line-height: 0.9;
            letter-spacing: -0.02em;
            cursor: pointer;
          }

          & .under-line {
            width: 0%;
            height: 4px;
            transition: 100ms;
            background-color: $yellow-color;
          }
        }
      }

      & .container-list {
        & .list-item {
          display: none;

          &.is-active {
            display: block;
          }

          & .icon {
            display: none;
          }

          & h2 {
            margin-top: 0px;
            margin-bottom: 0px;
            padding-bottom: 20.8px;
            font-family: "Recife Display", serif;
            font-size: 4em;
            line-height: 0.8311688311688312;
            font-weight: 400;
            letter-spacing: -0.02em;
          }

          & p {
            font-size: 16px;
            line-height: 1.3;
            overflow-wrap: break-word;
          }

          & .mobile-img {
            display: none;
          }
        }
      }
    }

    & .column:nth-child(2) {
      position: relative;
      border-radius: 7%;
      padding: 3em;

      & .wrapper-image {
        & .list-item {
          display: none;

          &.is-active {
            display: block;
          }

          & .img-photo {
            position: absolute;
            left: 0%;
            top: 0%;
            right: 0%;
            bottom: 0%;
            width: 100%;
            height: 100%;
            transform: scale(1.2);
            object-fit: cover;
          }

          & .icon {
            position: absolute;
            width: 6em;
            z-index: 9;
            transform: translate(-50%, -50%);
            max-width: 70%;
            top: 50%;
            left: 50%;
          }
        }
      }
      & p {
        z-index: 2;
        font-style: italic;
        font-family: "Recife Display", sans-serif;
        font-size: 2.25em;
        position: absolute;
        top: 40px;
        right: 45px;
      }
    }
  }
}

@media only screen and (max-width: 900px) {
  .container.section-1 {
    height: auto;

    & .row {
      & .column:nth-child(1) {
        & .wrapper-nav {
          display: none;
        }

        & .container-list {
          display: flex;
          flex-direction: column;
          row-gap: 5em;

          & .list-item {
            display: block;

            & h2,
            p {
              width: 60%;
            }

            & .icon {
              display: block;
              width: 2.9em;
              margin-bottom: 1.6em;
            }

            & .mobile-img {
              position: relative;
              display: block;
              width: 100%;
              margin-top: 2em;
              padding-top: 50%;

              & img {
                position: absolute;
                left: 0%;
                top: 0%;
                right: 0%;
                bottom: 0%;
                width: 100%;
                height: 100%;
                -o-object-fit: cover;
                object-fit: cover;
              }
            }
          }
        }
      }

      & .column:nth-child(2) {
        display: none;
      }
    }
  }
}
</style>
