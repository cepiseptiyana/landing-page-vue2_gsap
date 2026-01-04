<template>
  <div class="container section-2">
    <div class="stiky">
      <div class="progress-horizontal"></div>

      <div class="wrapper-image">
        <div class="list">
          <div
            v-for="(data, index) in data_section"
            :key="index"
            class="list-item"
          >
            <div class="img-overflow">
              <img :src="data.image" alt="" />
            </div>
          </div>
        </div>
      </div>

      <div class="wrapper-content">
        <p class="number"><span>1</span> — <span>3</span></p>
        <div class="list">
          <div
            v-for="(data, index) in data_section"
            :key="index"
            class="list-item"
          >
            <h2>{{ data.heading }}</h2>
          </div>
        </div>
        <a href="#" class="button w-button">Learn More</a>
      </div>
    </div>
  </div>
</template>

<script>
import { gsap } from "gsap";
import ScrollTrigger from "gsap/ScrollTrigger";
gsap.registerPlugin(ScrollTrigger);

import Img from "@/assets/images/table_xl.jpg";
import Wall from "@/assets/images/wall_xl.jpg";
import Men from "@/assets/images/men_xl.jpg";

export default {
  name: "section-2",
  props: {
    msg: String,
  },
  data() {
    return {
      data_section: [
        {
          image: Img,
          heading: "plan",
        },
        {
          image: Wall,
          heading: "design",
        },
        {
          image: Men,
          heading: "build",
        },
      ],
    };
  },

  mounted() {
    const section = document.querySelector(".section-2");

    const el_progress_hz = section.querySelector(".stiky .progress-horizontal");
    const el_scaleSize_img = section.querySelectorAll(
      ".stiky .wrapper-image .list .list-item"
    );
    const el_number = section.querySelector(
      ".stiky .wrapper-content .number span:first-child"
    );

    const el_wrapper_heading = section.querySelector(
      ".stiky .wrapper-content .list"
    );

    const el_heading = section.querySelectorAll(
      ".stiky .wrapper-content .list .list-item"
    );

    const data_list = this.data_section;
    let lastIndex = -1;

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

        // TRIGGER HORIZONTAL PROGRESS
        el_progress_hz.style.width = `${self.progress * 100}%`;

        // TRIGGER IMAGE
        el_scaleSize_img.forEach((el, i) => {
          let scale = 0;

          if (i < index) {
            // image sebelumnya tetap full
            scale = 1;
          } else if (i === index) {
            // image aktif mengikuti scroll
            scale = localProgress;
          } else {
            // scale awal
            scale = 0;
          }

          el.classList.toggle("is-active", i <= index);
          el.style.transform = `scale(${scale})`;
        });

        // TRIGGER NUMBER
        const jumlah_halaman = data_list.length;
        //   // TRIGGER JUMLAH HALAMAN
        for (let i = 0; i <= jumlah_halaman; i++) {
          if (index == i) {
            el_number.innerHTML = i + 1;
          }
        }

        // TRIGGER HEADING
        // cek kalau index berubah
        function scrollToCenter(container, element) {
          // posisi element relatif container
          const scrollTop =
            container.scrollTop +
            element.getBoundingClientRect().top -
            container.getBoundingClientRect().top -
            container.clientHeight / 2 +
            element.offsetHeight / 2;

          container.scrollTop = scrollTop;
        }

        if (index !== lastIndex) {
          lastIndex = index;
          scrollToCenter(el_wrapper_heading, el_heading[index]);
        }
      },
    });
  },
};
</script>

<style lang="scss" scoped>
@import "@/assets/styles/variables";

.container.section-2 {
  background-color: $dark-color;
  position: relative;
  text-align: center;

  & .stiky {
    position: -webkit-sticky;
    position: sticky;
    top: 0px;
    z-index: 2;
    display: -webkit-box;
    display: -webkit-flex;
    display: -ms-flexbox;
    display: flex;
    overflow: hidden;
    height: 100vh;
    -webkit-box-orient: vertical;
    -webkit-box-direction: normal;
    -webkit-flex-direction: column;
    -ms-flex-direction: column;
    flex-direction: column;
    -webkit-box-pack: center;
    -webkit-justify-content: center;
    -ms-flex-pack: center;
    justify-content: center;
    -webkit-box-align: center;
    -webkit-align-items: center;
    -ms-flex-align: center;
    align-items: center;

    & .progress-horizontal {
      position: absolute;
      top: 0%;
      left: 0%;
      right: 0%;
      z-index: 9999;
      width: 0%;
      height: 5px;
      background-color: #c6fb50;
    }

    & .wrapper-image {
      position: absolute;
      left: 0%;
      top: 0%;
      right: 0%;
      bottom: 0%;
      width: 100%;
      height: 100%;

      & .list {
        position: absolute;
        left: 0%;
        top: 0%;
        right: 0%;
        bottom: 0%;
        width: 100%;
        height: 100vh;

        & .list-item {
          position: absolute;
          left: 0%;
          top: 0%;
          right: 0%;
          bottom: 0%;
          width: 100%;
          height: 100%;
          display: none;

          &.is-active {
            display: block;
          }

          & .img-overflow {
            position: relative;
            overflow: hidden;
            width: 100%;
            height: 100%;
            //   -webkit-transform: scale(0);
            //   -ms-transform: scale(0);
            //   transform: scale(0);

            & img {
              width: 100%;
              height: 100%;
              //   -webkit-transform: scale(2);
              //   -ms-transform: scale(2);
              //   transform: scale(2);
              -o-object-fit: cover;
              object-fit: cover;
            }
          }
        }
      }
    }

    & .wrapper-content {
      position: relative;
      z-index: 2;
      display: flex;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap: 40px;

      & .number {
        position: relative;
        z-index: 2;
        font-size: 1.13em;
      }

      & .list {
        height: 12.2em;
        overflow-y: scroll;
        scroll-behavior: smooth;

        &::-webkit-scrollbar {
          width: 0;
        }

        .list-item {
          & h2 {
            font-style: italic;
            padding-top: 0.05em;
            padding-bottom: 0.22em;
            font-family: "Recife Display", serif;
            font-size: 11.38em;
            line-height: 0.8;
            font-weight: 400;
            letter-spacing: -0.05em;
          }
        }
      }

      & .button {
        text-decoration: none;
        padding: 0.1em 1.7em;
        border-style: solid;
        border-width: 1.5px;
        border-color: #c6fb50;
        border-radius: 100vw;
        background-color: transparent;
        color: #c6fb50;
        font-size: 1.13em;

        border: 1px solid $yellow-color;

        &:hover {
          background-color: #c6fb50;
          color: #111606;
        }
      }
    }
  }
}
</style>
