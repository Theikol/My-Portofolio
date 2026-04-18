<script setup>
import { ref, onMounted, onUnmounted, computed } from 'vue';

// ─── DATA PROFIL ──────────────────────────────────────────────
const profil = ref({
  namaSingkat:      "Adrian",
  namaPanggilan:    "Adrian",
  namaLengkap:      "Adrian Haikal",
  deskripsiPendek:  "I am a fresh graduate with a GPA of 3.X who is interested in the field of frontend development.",
  email:            "ikoladeian@gmail.com",
  // Ganti path di bawah dengan path foto Anda (misal: '/assets/foto.jpg')
  foto:             null,
  statusTersedia:   true,
});

const menu = ref([
  { id: 1, nama: "Beranda",  href: "#beranda"  },
  { id: 2, nama: "Tentang",  href: "#tentang"  },
  { id: 3, nama: "Proyek",   href: "#proyek"   },
  { id: 4, nama: "Kontak",   href: "#kontak"   },
]);

const keahlian = ref([
  { nama: "Vue.js",            ikon: "fa-brands fa-vuejs",   warna: "clr-vue"   },
{ nama: "Nuxt.js",           ikon: "fa-brands fa-vuejs",   warna: "clr-nuxt"  },
{ nama: "JavaScript (ES6+)", ikon: "fa-brands fa-js",      warna: "clr-js"    },
{ nama: "HTML & CSS",        ikon: "fa-brands fa-html5",   warna: "clr-html"  },
{ nama: "Tailwind CSS",      ikon: "fa-solid fa-wind",     warna: "clr-tw"    },
{ nama: "Git & GitHub",      ikon: "fa-brands fa-github",  warna: "clr-git"   },

{ nama: "Go (Golang)",       ikon: "fa-solid fa-code",     warna: "clr-go"    },
{ nama: "MySQL",             ikon: "fa-solid fa-database", warna: "clr-mysql" },
{ nama: "Ubuntu",            ikon: "fa-brands fa-ubuntu",  warna: "clr-ubuntu"},
{ nama: "PHP",               ikon: "fa-brands fa-php",     warna: "clr-php"   },
]);

const proyek = ref([
  {
    id: 1,
    judul:      "Activity Monitoring System",
    deskripsi:  "Program monitoring dan pemantauan aktivitas yang terintegrasi exproler dengan webcam dan screen recording untuk mendeteksi aktivitas pengguna",
    teknologi:  ["Python 3.10", "Open CV", "MSS", "Pywin32", "Numpy"],
    github: "https://github.com/Theikol/WhatIsHeDoing-.git",
    link:   "#",
  },
  {
    id: 2,
    judul:      "Website Index Movies & Trending Movies",
    deskripsi:  "Kloning antarmuka web streaming dengan TMDB API. Pencarian cerdas, trailer, dan daftar tontonan.",
    teknologi:  ["React", "Gin", "MySQL", "TMDB_API", "Rest_API"],
    github: "#",
    link:   "https://ikolcineverse.online",
  },
  {
    id: 3,
    judul:      "Dashboard Management Keuangan",
    deskripsi:  "Sistem pencatatan pemasukan & pengeluaran serta analisis keuangan dengan php native",
    teknologi:  ["Xampp Environment with PHP Native"],
    github: "https://github.com/Theikol/FinancialManagement.git",
    link:   "#",
  },
  // Tambahkan proyek lain di sini{
  //  id: 4,
  //  judul:      "Website Undangan Pernikahan",
  // deskripsi:  "Undangan digital interaktif dengan fitur RSVP, buku tamu, dan galeri foto animasi yang mulus.",
  // teknologi:  ["Vue 3", "Tailwind", "GSAP"],
  //  github: "#",
  //  link:   "#",
  //},
]);

const sosial = ref([
  { nama: "GitHub",    url: "https://github.com/Theikol",    ikon: "fa-brands fa-github"      },
  { nama: "LinkedIn",  url: "https://linkedin.com/in/adrian-haikal-60aa03320",  ikon: "fa-brands fa-linkedin-in" },
  { nama: "Instagram", url: "https://instagram.com/adrianhaikall_", ikon: "fa-brands fa-instagram"   },
]);

// ─── UI STATE ─────────────────────────────────────────────────
const menuTerbuka   = ref(false);
const isScrolled    = ref(false);
const activeSection = ref('beranda');

const mailtoLink = computed(() => `mailto:${profil.value.email}`);

// ─── SCROLL HANDLER ──────────────────────────────────────────
const handleScroll = () => {
  isScrolled.value = window.scrollY > 20;
  const sections = ['beranda', 'tentang', 'proyek', 'kontak'];
  for (const id of sections) {
    const el = document.getElementById(id);
    if (el) {
      const { top } = el.getBoundingClientRect();
      if (top <= 80) activeSection.value = id;
    }
  }
};

// ─── INTERSECTION OBSERVER (scroll-reveal) ────────────────────
let observer = null;
const initReveal = () => {
  observer = new IntersectionObserver(
    (entries) => {
      entries.forEach(e => {
        if (e.isIntersecting) {
          e.target.classList.add('revealed');
          observer.unobserve(e.target);
        }
      });
    },
    { threshold: 0.12, rootMargin: '0px 0px -40px 0px' }
  );
  document.querySelectorAll('.reveal').forEach(el => observer.observe(el));
};

// ─── LIFECYCLE ───────────────────────────────────────────────
onMounted(() => {
  window.addEventListener('scroll', handleScroll, { passive: true });
  // Tunda sedikit agar DOM render sempurna sebelum observe
  setTimeout(initReveal, 100);
});

onUnmounted(() => {
  window.removeEventListener('scroll', handleScroll);
  observer?.disconnect();
});
</script>

<template>
  <div class="site-wrapper">

    <!-- ░░ BACKGROUND GRID DECORATION ░░ -->
    <div class="bg-grid" aria-hidden="true"></div>

    <!-- ═══════════════════════════════════════
         NAVIGASI
    ════════════════════════════════════════ -->
    <nav class="navbar" :class="{ scrolled: isScrolled }">
      <div class="container nav-inner">

        <!-- Logo -->
        <a href="#beranda" class="logo">
          {{ profil.namaSingkat }}<span class="accent">.</span>
        </a>

        <!-- Menu Desktop -->
        <ul class="nav-links" role="list">
          <li v-for="item in menu" :key="item.id">
            <a
              :href="item.href"
              class="nav-link"
              :class="{ active: activeSection === item.href.slice(1) }"
            >
              {{ item.nama }}
            </a>
          </li>
        </ul>

        <!-- Tombol Mobile -->
        <button
          class="menu-btn"
          :class="{ open: menuTerbuka }"
          @click="menuTerbuka = !menuTerbuka"
          :aria-expanded="menuTerbuka"
          aria-label="Toggle menu"
        >
          <span></span><span></span><span></span>
        </button>
      </div>

      <!-- Menu Mobile -->
      <Transition name="slide-down">
        <div v-show="menuTerbuka" class="mobile-menu">
          <a
            v-for="item in menu"
            :key="item.id"
            :href="item.href"
            class="mobile-link"
            @click="menuTerbuka = false"
          >{{ item.nama }}</a>
        </div>
      </Transition>
    </nav>

    <!-- ═══════════════════════════════════════
         BERANDA — HERO
    ════════════════════════════════════════ -->
    <section id="beranda" class="hero">
      <div class="container hero-inner">

        <!-- Teks Hero -->
        <div class="hero-text">
          <p class="hero-greeting reveal" style="--delay:.1s">
            <span class="dot"></span> Hello!, My Name is
          </p>
          <h1 class="hero-name reveal" style="--delay:.25s">
            {{ profil.namaLengkap }}<span class="accent">.</span>
          </h1>
          <h2 class="hero-sub reveal" style="--delay:.4s">
            I'am a &nbsp;<em>Full Stack Developer</em>.
          </h2>
          <p class="hero-desc reveal" style="--delay:.55s">
            {{ profil.deskripsiPendek }}
          </p>

          <!-- Badge Tersedia -->
          <div v-if="profil.statusTersedia" class="badge reveal" style="--delay:.65s">
            <span class="badge-dot"></span>
            I am open to work!
          </div>

          <div class="hero-cta reveal" style="--delay:.7s">
            <a href="#proyek" class="btn-primary">
              Lihat Karya <!--<i class="fa-solid fa-arrow-right"></i>-->
            </a>
            <a :href="mailtoLink" class="btn-outline">
              Hubungi Saya
            </a>
          </div>
        </div>

        <!-- ✦ FOTO PROFIL — Ganti `profil.foto` dengan path gambar Anda ✦ -->
        <div class="hero-photo reveal" style="--delay:.3s">
          <div class="photo-frame">
            <img
              v-if="profil.foto"
              :src="profil.foto"
              :alt="`Foto ${profil.namaLengkap}`"
              class="photo-img"
            />
            <!-- Placeholder tampil jika foto belum diisi -->
            <div v-else class="photo-placeholder">
              <i class="fa-regular fa-user photo-icon"></i>
              <p class="photo-hint">Foto Anda di sini</p>
              <code class="photo-code">profil.foto = '/assets/foto.jpg'</code>
            </div>
            <div class="photo-border" aria-hidden="true"></div>
          </div>
        </div>

      </div>
    </section>

    <!-- ═══════════════════════════════════════
         TENTANG SAYA
    ════════════════════════════════════════ -->
    <section id="tentang" class="section bg-alt">
      <div class="container">
        <div class="section-heading reveal">
          <span class="section-num">01.</span>
          <h2 class="section-title">Tentang Saya</h2>
          <div class="section-line"></div>
        </div>

        <div class="about-grid">
          <!-- Teks -->
          <div class="about-text">
            <p class="reveal" style="--delay:.1s">
              Halo! Nama saya <strong>{{ profil.namaPanggilan }}</strong> dan saya sangat
              tertarik dalam menciptakan hal-hal yang hidup di internet. Minat saya
              pada pengembangan web dan ini adalah hobi yang saya tekuni sejak masa perkuliahan, 
            </p>
            <p class="reveal" style="--delay:.2s">
              Saat ini, fokus utama saya adalah membangun produk yang aksesibel,
              inklusif, dan memberikan pengalaman digital memuaskan menggunakan
              <span class="accent-text">Vue.js, React</span> dan ekosistemnya.
            </p>
          </div>

          <!-- Keahlian -->
          <div class="skill-card reveal" style="--delay:.15s">
            <h3 class="skill-title">
              <i class="fa-solid fa-code accent-text"></i>
              Teknologi yang saya kuasai
            </h3>
            <ul class="skill-grid" role="list">
              <li
                v-for="(skill, i) in keahlian"
                :key="skill.nama"
                class="skill-item reveal"
                :style="`--delay:${0.1 + i * 0.07}s`"
              >
                <i :class="[skill.ikon, skill.warna]"></i>
                <span>{{ skill.nama }}</span>
              </li>
            </ul>
          </div>
        </div>
      </div>
    </section>

    <!-- ═══════════════════════════════════════
         PROYEK
    ════════════════════════════════════════ -->
    <section id="proyek" class="section">
      <div class="container">
        <div class="section-heading reveal">
          <span class="section-num">02.</span>
          <h2 class="section-title">Hal yang Telah Saya Buat</h2>
          <div class="section-line"></div>
        </div>

        <div class="projects-grid">
          <article
            v-for="(p, i) in proyek"
            :key="p.id"
            class="project-card reveal"
            :style="`--delay:${i * 0.1}s`"
          >
            <div class="project-top">
              <i class="fa-regular fa-folder project-folder"></i>
              <div class="project-links">
                <a v-if="p.github" :href="p.github" target="_blank" rel="noopener" aria-label="GitHub">
                  <i class="fa-brands fa-github"></i>
                </a>
                <a v-if="p.link" :href="p.link" target="_blank" rel="noopener" aria-label="Live demo">
                  <i class="fa-solid fa-arrow-up-right-from-square"></i>
                </a>
              </div>
            </div>
            <h3 class="project-title">{{ p.judul }}</h3>
            <p class="project-desc">{{ p.deskripsi }}</p>
            <ul class="project-tech" role="list">
              <li v-for="tech in p.teknologi" :key="tech">{{ tech }}</li>
            </ul>
          </article>
        </div>
      </div>
    </section>

    <!-- ═══════════════════════════════════════
         KONTAK
    ════════════════════════════════════════ -->
    <section id="kontak" class="section bg-alt">
      <div class="container">
        <div class="contact-wrap reveal">
          <p class="contact-eyebrow">
            <span class="section-num">03.</span> Apa Selanjutnya?
          </p>
          <h2 class="contact-title">Mari Terhubung</h2>
          <p class="contact-desc">
            Saat ini saya terbuka untuk peluang baru. Kotak masuk saya selalu terbuka.
            Baik Anda punya pertanyaan, tawaran pekerjaan, atau sekadar ingin menyapa,
            saya akan berusaha membalas secepat mungkin!
          </p>
          <a :href="`mailto:${profil.email}`" class="btn-outline btn-lg reveal" style="--delay:.2s">
            Kirim Pesan <i class="fa-regular fa-paper-plane"></i>
          </a>
        </div>
      </div>
    </section>

    <!-- ═══════════════════════════════════════
         FOOTER
    ════════════════════════════════════════ -->
    <footer class="footer">
      <div class="footer-social md-hide">
        <a
          v-for="s in sosial" :key="s.nama"
          :href="s.url" target="_blank" rel="noopener"
          :aria-label="s.nama"
        ><i :class="s.ikon"></i></a>
      </div>
      <p>© 2026  <span class="accent-text">{{ profil.namaPanggilan }}</span> All Rights Reserved </p>
      <p class="footer-sub">Vue 3 + Tailwind CSS</p>
    </footer>

    <!-- Sidebar Sosial (Desktop) -->
    <aside class="sidebar-social" aria-label="Social links">
      <ul role="list">
        <li v-for="s in sosial" :key="s.nama">
          <a :href="s.url" target="_blank" rel="noopener" :aria-label="s.nama">
            <i :class="s.ikon"></i>
          </a>
        </li>
      </ul>
    </aside>

    <!-- Sidebar Email (Desktop) -->
    <aside class="sidebar-email" aria-label="Email">
      <a :href="mailtoLink" class="email-link">{{ "ikoladrian@gmail.com" }}</a>
    </aside>

  </div>
</template>

<style>
/* ═══════════════════════════════════════════════════════════════
   FONT IMPORT
════════════════════════════════════════════════════════════════ */
@import url('https://fonts.googleapis.com/css2?family=Syne:wght@400;600;700;800&family=DM+Sans:ital,wght@0,300;0,400;0,500;1,400&family=DM+Mono:wght@400;500&display=swap');

/* ═══════════════════════════════════════════════════════════════
   CSS VARIABLES
════════════════════════════════════════════════════════════════ */
:root {
  --bg:        #0a0e17;
  --bg-alt:    #0f1521;
  --surface:   #141b27;
  --border:    rgba(255,255,255,.07);
  --accent:    #00e5a0;
  --accent-dim:#00e5a033;
  --text:      #c8d3e0;
  --text-dim:  #637087;
  --heading:   #eef2f7;
  --font-head: 'Syne', sans-serif;
  --font-body: 'DM Sans', sans-serif;
  --font-mono: 'DM Mono', monospace;
  --radius:    12px;
  --trans:     .25s ease;
  /* keahlian warna */
  --clr-vue:   #42d392;
  --clr-nuxt:  #00dc82;
  --clr-js:    #f7df1e;
  --clr-html:  #e44d26;
  --clr-tw:    #38bdf8;
  --clr-git:   #d1d5db;
}

/* ═══════════════════════════════════════════════════════════════
   RESET & BASE
════════════════════════════════════════════════════════════════ */
*, *::before, *::after { box-sizing: border-box; margin: 0; padding: 0; }

html { scroll-behavior: smooth; font-size: 16px; }

body {
  background: var(--bg);
  color: var(--text);
  font-family: var(--font-body);
  line-height: 1.7;
  -webkit-font-smoothing: antialiased;
}

::selection { background: var(--accent); color: #000; }

::-webkit-scrollbar       { width: 6px; }
::-webkit-scrollbar-track { background: var(--bg); }
::-webkit-scrollbar-thumb { background: #2a3547; border-radius: 3px; }
::-webkit-scrollbar-thumb:hover { background: var(--accent); }

/* ═══════════════════════════════════════════════════════════════
   LAYOUT
════════════════════════════════════════════════════════════════ */
.site-wrapper { position: relative; min-height: 100vh; }
.container    { max-width: 1100px; margin-inline: auto; padding-inline: clamp(1.25rem, 5vw, 2rem); }

/* Background grid texture */
.bg-grid {
  position: fixed; inset: 0; z-index: 0; pointer-events: none;
  background-image:
    linear-gradient(rgba(0,229,160,.025) 1px, transparent 1px),
    linear-gradient(90deg, rgba(0,229,160,.025) 1px, transparent 1px);
  background-size: 60px 60px;
  mask-image: radial-gradient(ellipse 80% 80% at 50% 20%, black, transparent);
}

/* ═══════════════════════════════════════════════════════════════
   ANIMASI — SCROLL REVEAL
════════════════════════════════════════════════════════════════ */
.reveal {
  opacity: 0;
  transform: translateY(28px);
  transition: opacity .6s cubic-bezier(.25,.8,.25,1) var(--delay, 0s),
              transform .6s cubic-bezier(.25,.8,.25,1) var(--delay, 0s);
}
.reveal.revealed {
  opacity: 1;
  transform: none;
}

/* ═══════════════════════════════════════════════════════════════
   NAVBAR
════════════════════════════════════════════════════════════════ */
.navbar {
  position: fixed; top: 0; width: 100%; z-index: 100;
  padding: .875rem 0;
  transition: background var(--trans), box-shadow var(--trans), backdrop-filter var(--trans);
}
.navbar.scrolled {
  background: rgba(10,14,23,.88);
  backdrop-filter: blur(16px);
  box-shadow: 0 1px 0 var(--border);
}
.nav-inner {
  display: flex; align-items: center; justify-content: space-between;
}
.logo {
  font-family: var(--font-head);
  font-weight: 800; font-size: 1.5rem;
  color: var(--heading); text-decoration: none; letter-spacing: -.02em;
  transition: color var(--trans);
}
.logo:hover { color: var(--accent); }

.nav-links { list-style: none; display: flex; gap: 2rem; }
.nav-link  {
  font-size: .875rem; font-weight: 500; color: var(--text);
  text-decoration: none; letter-spacing: .02em;
  position: relative; padding-bottom: 2px;
  transition: color var(--trans);
}
.nav-link::after {
  content: ''; position: absolute; bottom: 0; left: 0;
  width: 0; height: 1.5px; background: var(--accent);
  transition: width var(--trans);
}
.nav-link:hover, .nav-link.active { color: var(--accent); }
.nav-link:hover::after, .nav-link.active::after { width: 100%; }

/* Hamburger */
.menu-btn {
  display: none; flex-direction: column; gap: 5px;
  background: none; border: none; cursor: pointer; padding: 4px;
}
.menu-btn span {
  display: block; width: 22px; height: 2px;
  background: var(--text); border-radius: 2px;
  transform-origin: center;
  transition: transform .3s ease, opacity .3s ease, background .3s ease;
}
.menu-btn.open span:nth-child(1) { transform: translateY(7px) rotate(45deg); }
.menu-btn.open span:nth-child(2) { opacity: 0; }
.menu-btn.open span:nth-child(3) { transform: translateY(-7px) rotate(-45deg); }

.mobile-menu {
  display: flex; flex-direction: column;
  background: rgba(10,14,23,.97);
  backdrop-filter: blur(20px);
  border-top: 1px solid var(--border);
  padding: 1rem 0;
}
.mobile-link {
  padding: .75rem 1.5rem; color: var(--text);
  text-decoration: none; font-weight: 500;
  transition: color var(--trans), padding-left var(--trans);
}
.mobile-link:hover { color: var(--accent); padding-left: 2rem; }

/* Slide-down transition */
.slide-down-enter-active, .slide-down-leave-active {
  transition: transform .3s ease, opacity .3s ease;
}
.slide-down-enter-from, .slide-down-leave-to {
  transform: translateY(-12px); opacity: 0;
}

/* ═══════════════════════════════════════════════════════════════
   HERO
════════════════════════════════════════════════════════════════ */
.hero {
  min-height: 100svh;
  display: flex; align-items: center;
  padding-top: 5rem; padding-bottom: 3rem;
  position: relative;
}

/* Radial glow behind hero */
.hero::before {
  content: ''; position: absolute;
  top: -10%; left: 50%; transform: translateX(-50%);
  width: 700px; height: 500px;
  background: radial-gradient(ellipse, rgba(0,229,160,.06) 0%, transparent 70%);
  pointer-events: none;
}

.hero-inner {
  display: grid;
  grid-template-columns: 1fr 1fr;
  gap: 4rem; align-items: center;
}

.hero-greeting {
  display: flex; align-items: center; gap: .6rem;
  font-family: var(--font-mono); font-size: .875rem;
  color: var(--accent); margin-bottom: 1rem;
}
.dot {
  width: 8px; height: 8px; border-radius: 50%;
  background: var(--accent); flex-shrink: 0;
  animation: pulse 2s infinite;
}
@keyframes pulse {
  0%, 100% { box-shadow: 0 0 0 0 rgba(0,229,160,.5); }
  50%       { box-shadow: 0 0 0 6px rgba(0,229,160,0); }
}

.hero-name {
  font-family: var(--font-head);
  font-size: clamp(2.8rem, 6vw, 4.5rem);
  font-weight: 800; line-height: 1.05;
  color: var(--heading); letter-spacing: -.03em;
  margin-bottom: .5rem;
}

.hero-sub {
  font-family: var(--font-head);
  font-size: clamp(1.6rem, 3.5vw, 2.6rem);
  font-weight: 700; color: var(--text-dim);
  margin-bottom: 1.25rem; letter-spacing: -.02em;
}
.hero-sub em { font-style: italic; color: var(--accent); }

.hero-desc {
  font-size: 1.0625rem; color: var(--text);
  max-width: 500px; margin-bottom: 1.5rem; line-height: 1.8;
}

/* Badge */
.badge {
  display: inline-flex; align-items: center; gap: .5rem;
  background: rgba(0,229,160,.08); border: 1px solid rgba(0,229,160,.2);
  color: var(--accent); font-size: .8rem; font-family: var(--font-mono);
  padding: .35rem .9rem; border-radius: 999px;
  margin-bottom: 2rem;
}
.badge-dot {
  width: 7px; height: 7px; border-radius: 50%;
  background: var(--accent);
  animation: pulse 2s infinite;
}

.hero-cta { display: flex; gap: 1rem; flex-wrap: wrap; }

/* ─── Foto Profil ──────────────────────────────────────────── */
.hero-photo { display: flex; justify-content: center; }

.photo-frame {
  position: relative; width: 300px; height: 340px;
}

/* Foto asli */
.photo-img {
  width: 100%; height: 100%;
  object-fit: cover; object-position: top;
  border-radius: var(--radius);
  display: block;
  filter: grayscale(15%);
  transition: filter var(--trans);
}
.photo-frame:hover .photo-img { filter: grayscale(0); }

/* Placeholder */
.photo-placeholder {
  width: 100%; height: 100%;
  border-radius: var(--radius);
  background: var(--surface);
  border: 1.5px dashed rgba(0,229,160,.35);
  display: flex; flex-direction: column;
  align-items: center; justify-content: center; gap: .75rem;
  text-align: center; padding: 1.5rem;
}
.photo-icon { font-size: 3rem; color: var(--text-dim); }
.photo-hint { font-size: .875rem; color: var(--text-dim); }
.photo-code {
  font-family: var(--font-mono); font-size: .7rem;
  color: var(--accent); background: var(--accent-dim);
  padding: .3rem .6rem; border-radius: 6px;
  word-break: break-all;
}

/* Bingkai dekoratif */
.photo-border {
  position: absolute;
  inset: -10px -10px auto auto;
  width: calc(100% + 10px); height: calc(100% + 10px);
  border: 1.5px solid var(--accent);
  border-radius: var(--radius);
  z-index: -1;
  transition: transform var(--trans);
}
.photo-frame:hover .photo-border { transform: translate(4px, -4px); }

/* ═══════════════════════════════════════════════════════════════
   SECTIONS
════════════════════════════════════════════════════════════════ */
.section     { padding: 6rem 0; position: relative; }
.bg-alt      { background: var(--bg-alt); }

.section-heading {
  display: flex; align-items: center; gap: 1rem; margin-bottom: 3.5rem;
}
.section-num   { font-family: var(--font-mono); color: var(--accent); font-size: .875rem; }
.section-title {
  font-family: var(--font-head); font-weight: 700;
  font-size: clamp(1.5rem, 3vw, 2rem); color: var(--heading);
  white-space: nowrap;
}
.section-line  { flex-grow: 1; height: 1px; background: var(--border); max-width: 280px; }

/* ─── Tentang ──────────────────────────────────────────────── */
.about-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 3.5rem; align-items: start; }
.about-text  p { color: var(--text); line-height: 1.85; margin-bottom: 1.25rem; font-size: 1.0625rem; }
.accent-text   { color: var(--accent); font-weight: 600; }

.skill-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 2rem;
}
.skill-title {
  font-family: var(--font-head); font-weight: 700;
  color: var(--heading); font-size: 1rem; margin-bottom: 1.5rem;
  display: flex; align-items: center; gap: .6rem;
}
.skill-grid { display: grid; grid-template-columns: 1fr 1fr; gap: .875rem; list-style: none; }
.skill-item {
  display: flex; align-items: center; gap: .625rem;
  color: var(--text); font-size: .9375rem; font-weight: 500;
}
.skill-item i { font-size: 1.125rem; width: 1.25rem; text-align: center; }
/* Warna dinamis via class */
.clr-vue  { color: var(--clr-vue);  }
.clr-nuxt { color: var(--clr-nuxt); }
.clr-js   { color: var(--clr-js);   }
.clr-html { color: var(--clr-html); }
.clr-tw   { color: var(--clr-tw);   }
.clr-git  { color: var(--clr-git);  }

/* ─── Proyek ───────────────────────────────────────────────── */
.projects-grid {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(280px, 1fr));
  gap: 1.5rem;
}
.project-card {
  background: var(--surface); border: 1px solid var(--border);
  border-radius: var(--radius); padding: 2rem;
  display: flex; flex-direction: column; gap: .875rem;
  transition: border-color var(--trans), transform var(--trans), box-shadow var(--trans);
}
.project-card:hover {
  border-color: rgba(0,229,160,.35);
  transform: translateY(-6px);
  box-shadow: 0 20px 40px rgba(0,0,0,.3);
}

.project-top { display: flex; justify-content: space-between; align-items: center; }
.project-folder { font-size: 2.25rem; color: var(--accent); }

.project-links { display: flex; gap: 1rem; }
.project-links a { color: var(--text-dim); font-size: 1.125rem; transition: color var(--trans); text-decoration: none; }
.project-links a:hover { color: var(--accent); }

.project-title {
  font-family: var(--font-head); font-weight: 700;
  font-size: 1.1875rem; color: var(--heading); line-height: 1.3;
  transition: color var(--trans);
}
.project-card:hover .project-title { color: var(--accent); }

.project-desc { color: var(--text-dim); font-size: .9375rem; line-height: 1.7; flex-grow: 1; }

.project-tech { display: flex; flex-wrap: wrap; gap: .5rem .875rem; list-style: none; }
.project-tech li { font-family: var(--font-mono); font-size: .75rem; color: var(--text-dim); }

/* ─── Kontak ───────────────────────────────────────────────── */
.contact-wrap     { text-align: center; max-width: 580px; margin-inline: auto; }
.contact-eyebrow  { font-family: var(--font-mono); font-size: .875rem; color: var(--accent); margin-bottom: .75rem; }
.contact-title    {
  font-family: var(--font-head); font-weight: 800;
  font-size: clamp(2rem, 5vw, 3.25rem); color: var(--heading);
  letter-spacing: -.03em; margin-bottom: 1.25rem;
}
.contact-desc     { color: var(--text); line-height: 1.8; margin-bottom: 2.5rem; }

/* ─── Tombol ───────────────────────────────────────────────── */
.btn-primary, .btn-outline {
  display: inline-flex; align-items: center; gap: .5rem;
  font-family: var(--font-head); font-weight: 700; font-size: .9375rem;
  border-radius: 8px; padding: .75rem 1.75rem;
  cursor: pointer; text-decoration: none;
  transition: all var(--trans);
}
.btn-primary {
  background: var(--accent); color: #000;
  border: 2px solid var(--accent);
  box-shadow: 0 0 0 0 var(--accent-dim);
}
.btn-primary:hover {
  background: #00ffb3; border-color: #00ffb3;
  box-shadow: 0 0 20px var(--accent-dim);
  transform: translateY(-2px);
}
.btn-outline {
  background: transparent; color: var(--accent);
  border: 2px solid var(--accent);
}
.btn-outline:hover { background: var(--accent-dim); transform: translateY(-2px); }
.btn-lg { padding: .9rem 2.25rem; font-size: 1rem; }

/* ─── Footer ───────────────────────────────────────────────── */
.footer {
  padding: 2.5rem 1rem; text-align: center;
  border-top: 1px solid var(--border);
  font-size: .85rem; color: var(--text-dim);
}
.footer-social { display: flex; justify-content: center; gap: 1.5rem; margin-bottom: 1rem; }
.footer-social a { color: var(--text-dim); font-size: 1.125rem; text-decoration: none; transition: color var(--trans); }
.footer-social a:hover { color: var(--accent); }
.footer-sub { margin-top: .35rem; font-size: .75rem; }

/* ─── Sidebar Desktop ──────────────────────────────────────── */
.sidebar-social, .sidebar-email {
  display: none;
  position: fixed; bottom: 0; z-index: 50;
  flex-direction: column; align-items: center;
}

.sidebar-social { left: clamp(1rem, 3vw, 2.5rem); gap: 1.5rem; }
.sidebar-social::after,
.sidebar-email::after {
  content: ''; display: block;
  width: 1px; height: 6rem;
  background: var(--text-dim); margin-top: 1.5rem;
}
.sidebar-social ul { list-style: none; display: flex; flex-direction: column; gap: 1.25rem; }
.sidebar-social a   { color: var(--text-dim); font-size: 1.0625rem; text-decoration: none; transition: color var(--trans), transform var(--trans); }
.sidebar-social a:hover { color: var(--accent); transform: translateY(-3px); }

.sidebar-email { right: clamp(1rem, 3vw, 2.5rem); }
.email-link {
  writing-mode: vertical-rl; font-family: var(--font-mono);
  font-size: .75rem; letter-spacing: .12em; color: var(--text-dim);
  text-decoration: none; margin-bottom: 1.5rem;
  transition: color var(--trans);
}
.email-link:hover { color: var(--accent); }

/* ═══════════════════════════════════════════════════════════════
   RESPONSIVE
════════════════════════════════════════════════════════════════ */
@media (min-width: 768px) {
  .sidebar-social, .sidebar-email { display: flex; }
  .md-hide { display: none; }
  .menu-btn { display: none; }
  .nav-links { display: flex; }
}

@media (max-width: 767px) {
  .nav-links { display: none; }
  .menu-btn  { display: flex; }

  .hero-inner {
    grid-template-columns: 1fr;
    text-align: center;
  }
  /* Foto muncul sebelum teks di mobile */
  .hero-photo { order: -1; }
  .photo-frame { width: 200px; height: 220px; }
  .hero-cta    { justify-content: center; }
  .hero-desc   { margin-inline: auto; }
  .badge       { margin-inline: auto; }

  .about-grid  { grid-template-columns: 1fr; }
  .section-line{ display: none; }
}

@media (max-width: 480px) {
  .hero-name { font-size: 2.4rem; }
  .projects-grid { grid-template-columns: 1fr; }
}
</style>