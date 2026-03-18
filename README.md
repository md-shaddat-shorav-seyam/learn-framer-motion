# learn-framer-motion
Here’s a full **start-to-end guide to Framer Motion in React** with practical code and real-world uses.

First, one important update: **Framer Motion is now documented as “Motion for React” on motion.dev**. The React package is installed with `motion`, and Motion is officially compatible with **React 18.2+**. ([Motion][1])

---

# 1) What Framer Motion is

Framer Motion (now **Motion for React**) is a React animation library for:

* element enter/exit animation
* hover/tap/drag gestures
* layout animations
* scroll-triggered and scroll-linked animation
* orchestration with variants
* route/page transitions
* reorderable UI
* performant animation without React re-rendering every frame ([Motion][2])

In real projects, it is commonly used for:

* landing page hero animations
* animated cards and modals
* mobile nav drawers
* toast notifications
* page transitions
* dashboard widgets
* drag and reorder lists
* scroll reveal sections

---

# 2) Install in React

## Vite / React app

```bash
npm install motion
```

Then import from `motion/react`:

```jsx
import { motion } from "motion/react"
```

Motion’s official React install guide says **Vite needs no special setup**. ([Motion][3])

---

# 3) Your first animation

```jsx
import { motion } from "motion/react"

export default function App() {
  return (
    <div style={{ padding: 40 }}>
      <motion.div
        initial={{ opacity: 0, y: 40 }}
        animate={{ opacity: 1, y: 0 }}
        transition={{ duration: 0.6 }}
        style={{
          width: 200,
          height: 100,
          borderRadius: 16,
          background: "tomato"
        }}
      />
    </div>
  )
}
```

## What these props mean

* `initial`: starting state
* `animate`: target state
* `transition`: how it moves

This is the base pattern you’ll use everywhere.

---

# 4) Core concepts you must understand

## A. `motion` component

You can turn normal HTML tags into animated elements:

```jsx
<motion.div />
<motion.button />
<motion.h1 />
<motion.img />
<motion.svg />
```

Motion provides motion-enabled HTML and SVG elements. ([Motion][4])

---

## B. `initial`, `animate`, `exit`

```jsx
<motion.div
  initial={{ scale: 0.8, opacity: 0 }}
  animate={{ scale: 1, opacity: 1 }}
  exit={{ scale: 0.8, opacity: 0 }}
/>
```

Use:

* `initial` when it appears
* `animate` while visible
* `exit` when removed

---

## C. `transition`

```jsx
<motion.div
  animate={{ x: 200 }}
  transition={{ duration: 1, ease: "easeInOut" }}
/>
```

You can also use spring:

```jsx
<motion.div
  animate={{ x: 200 }}
  transition={{ type: "spring", stiffness: 120, damping: 12 }}
/>
```

Use spring for natural UI motion.

---

# 5) Common animation types

## A. Fade up

```jsx
<motion.div
  initial={{ opacity: 0, y: 30 }}
  animate={{ opacity: 1, y: 0 }}
  transition={{ duration: 0.5 }}
>
  Hello
</motion.div>
```

---

## B. Scale in

```jsx
<motion.div
  initial={{ scale: 0 }}
  animate={{ scale: 1 }}
  transition={{ type: "spring", stiffness: 180 }}
>
  Popup
</motion.div>
```

---

## C. Rotate

```jsx
<motion.div
  animate={{ rotate: 360 }}
  transition={{ duration: 1.2 }}
>
  Loading
</motion.div>
```

---

## D. Keyframes

```jsx
<motion.div
  animate={{ x: [0, 100, 50, 120, 0] }}
  transition={{ duration: 2 }}
/>
```

Keyframes are useful for bouncing indicators, loaders, and playful UI. Motion supports keyframes and transitions as part of its React animation system. ([Motion][5])

---

# 6) Hover, tap, and drag gestures

Gestures are one of the most useful real-world parts of Motion. ([Motion][1])

## Hover animation

```jsx
<motion.button
  whileHover={{ scale: 1.08 }}
  whileTap={{ scale: 0.96 }}
  style={{
    padding: "12px 20px",
    borderRadius: 12,
    border: "none",
    background: "black",
    color: "white",
    cursor: "pointer"
  }}
>
  Buy Now
</motion.button>
```

## Drag card

```jsx
<motion.div
  drag
  dragConstraints={{ left: -100, right: 100, top: -50, bottom: 50 }}
  whileDrag={{ scale: 1.05 }}
  style={{
    width: 180,
    height: 120,
    borderRadius: 16,
    background: "skyblue",
    display: "grid",
    placeItems: "center"
  }}
>
  Drag me
</motion.div>
```

Real-world uses:

* swipe cards
* draggable widgets
* sliders
* kanban items

---

# 7) Variants: the professional way to manage animations

When your app grows, writing `initial` and `animate` everywhere becomes messy.
Use **variants**.

```jsx
import { motion } from "motion/react"

const cardVariants = {
  hidden: { opacity: 0, y: 40 },
  visible: {
    opacity: 1,
    y: 0,
    transition: { duration: 0.5 }
  }
}

export default function App() {
  return (
    <motion.div
      variants={cardVariants}
      initial="hidden"
      animate="visible"
      style={{
        width: 240,
        padding: 20,
        borderRadius: 20,
        background: "#eee"
      }}
    >
      Animated Card
    </motion.div>
  )
}
```

## Parent-child orchestration with stagger

```jsx
import { motion } from "motion/react"

const container = {
  hidden: {},
  show: {
    transition: {
      staggerChildren: 0.15
    }
  }
}

const item = {
  hidden: { opacity: 0, y: 20 },
  show: { opacity: 1, y: 0 }
}

export default function App() {
  const items = ["HTML", "CSS", "JavaScript", "React"]

  return (
    <motion.ul
      variants={container}
      initial="hidden"
      animate="show"
      style={{ listStyle: "none", padding: 0 }}
    >
      {items.map((text) => (
        <motion.li
          key={text}
          variants={item}
          style={{
            marginBottom: 12,
            padding: 14,
            borderRadius: 12,
            background: "#f4f4f4"
          }}
        >
          {text}
        </motion.li>
      ))}
    </motion.ul>
  )
}
```

Variants and staggering are core orchestration features in Motion. ([Motion][1])

---

# 8) Animate on mount/unmount with `AnimatePresence`

This is essential for:

* modals
* mobile menus
* toast notifications
* tabs
* accordions
* route transitions

Motion’s `AnimatePresence` is the official way to animate elements when they leave the React tree. ([Motion][6])

## Modal example

```jsx
import { useState } from "react"
import { motion, AnimatePresence } from "motion/react"

export default function App() {
  const [open, setOpen] = useState(false)

  return (
    <div style={{ padding: 40 }}>
      <button onClick={() => setOpen(true)}>Open Modal</button>

      <AnimatePresence>
        {open && (
          <motion.div
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
            exit={{ opacity: 0 }}
            style={{
              position: "fixed",
              inset: 0,
              background: "rgba(0,0,0,0.5)",
              display: "grid",
              placeItems: "center"
            }}
            onClick={() => setOpen(false)}
          >
            <motion.div
              initial={{ scale: 0.8, opacity: 0, y: 40 }}
              animate={{ scale: 1, opacity: 1, y: 0 }}
              exit={{ scale: 0.8, opacity: 0, y: 40 }}
              transition={{ type: "spring", stiffness: 180, damping: 18 }}
              style={{
                width: 320,
                padding: 24,
                borderRadius: 20,
                background: "white"
              }}
              onClick={(e) => e.stopPropagation()}
            >
              <h2>Delete item?</h2>
              <p>This action cannot be undone.</p>
              <button onClick={() => setOpen(false)}>Close</button>
            </motion.div>
          </motion.div>
        )}
      </AnimatePresence>
    </div>
  )
}
```

### Important rule

`AnimatePresence` must stay mounted around the conditional element, otherwise exit animation won’t run. That behavior is explicitly documented by Motion. ([Motion][6])

---

# 9) Layout animations

This is one of the coolest features.

Add `layout` and Motion animates position/size changes automatically. Motion documents layout animation as a major feature of the library. ([Motion][1])

## Expanding card example

```jsx
import { useState } from "react"
import { motion } from "motion/react"

export default function App() {
  const [open, setOpen] = useState(false)

  return (
    <div style={{ padding: 40 }}>
      <motion.div
        layout
        onClick={() => setOpen(!open)}
        style={{
          width: open ? 420 : 240,
          padding: 20,
          borderRadius: 20,
          background: "#f3f3f3",
          cursor: "pointer"
        }}
      >
        <motion.h2 layout>Product Card</motion.h2>
        {open && (
          <motion.p
            initial={{ opacity: 0 }}
            animate={{ opacity: 1 }}
          >
            This extra content appears smoothly and the card resizes naturally.
          </motion.p>
        )}
      </motion.div>
    </div>
  )
}
```

Real-world use:

* expandable FAQ
* selected product card
* dashboard panels
* animated tab indicators

---

# 10) Shared layout animation

Very useful for tabs, galleries, and active indicators.

```jsx
import { useState } from "react"
import { motion } from "motion/react"

const tabs = ["Home", "About", "Contact"]

export default function App() {
  const [active, setActive] = useState("Home")

  return (
    <div style={{ display: "flex", gap: 12, padding: 40 }}>
      {tabs.map((tab) => (
        <button
          key={tab}
          onClick={() => setActive(tab)}
          style={{
            position: "relative",
            padding: "10px 18px",
            borderRadius: 999,
            border: "1px solid #ddd",
            background: "white"
          }}
        >
          {tab}
          {active === tab && (
            <motion.div
              layoutId="pill"
              style={{
                position: "absolute",
                inset: 0,
                borderRadius: 999,
                background: "#e8e8e8",
                zIndex: -1
              }}
            />
          )}
        </button>
      ))}
    </div>
  )
}
```

`layoutId` helps create shared transitions between matching elements.

---

# 11) Scroll-triggered animation

This is used constantly in real sites:

* reveal sections on scroll
* animate cards when they enter viewport
* timeline sections
* stats counters

## Reveal on scroll

```jsx
import { motion } from "motion/react"

function Section({ children }) {
  return (
    <motion.section
      initial={{ opacity: 0, y: 60 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.3 }}
      transition={{ duration: 0.6 }}
      style={{
        minHeight: "80vh",
        display: "grid",
        placeItems: "center",
        fontSize: 32
      }}
    >
      {children}
    </motion.section>
  )
}

export default function App() {
  return (
    <div>
      <Section>Hero</Section>
      <Section>Features</Section>
      <Section>Testimonials</Section>
      <Section>Pricing</Section>
    </div>
  )
}
```

Motion officially supports scroll animation patterns for React. ([Motion][1])

---

# 12) Scroll-linked animation with hooks

This is for progress bars, parallax, shrinking headers, and image transforms.

## Progress bar example

```jsx
import { motion, useScroll } from "motion/react"

export default function App() {
  const { scrollYProgress } = useScroll()

  return (
    <>
      <motion.div
        style={{
          scaleX: scrollYProgress,
          transformOrigin: "0% 50%",
          position: "fixed",
          top: 0,
          left: 0,
          right: 0,
          height: 6,
          background: "tomato"
        }}
      />
      <div style={{ height: "300vh", paddingTop: 40 }}>
        Scroll down
      </div>
    </>
  )
}
```

---

## Parallax effect with `useTransform`

```jsx
import { motion, useScroll, useTransform } from "motion/react"

export default function App() {
  const { scrollY } = useScroll()
  const y = useTransform(scrollY, [0, 600], [0, -150])

  return (
    <div style={{ height: "200vh" }}>
      <motion.div
        style={{
          y,
          height: 400,
          background: "linear-gradient(to right, #7dd3fc, #c084fc)"
        }}
      />
    </div>
  )
}
```

`useTransform` is Motion’s official way to derive new animated values from existing motion values. ([Motion][7])

---

# 13) Real-world project 1: animated navbar drawer

This is a practical mobile menu.

```jsx
import { useState } from "react"
import { motion, AnimatePresence } from "motion/react"

const menuVariants = {
  hidden: { x: "100%" },
  show: {
    x: 0,
    transition: { type: "spring", stiffness: 120, damping: 18 }
  },
  exit: { x: "100%" }
}

export default function App() {
  const [open, setOpen] = useState(false)

  return (
    <div>
      <header style={{ padding: 20, borderBottom: "1px solid #ddd" }}>
        <button onClick={() => setOpen(true)}>Menu</button>
      </header>

      <AnimatePresence>
        {open && (
          <>
            <motion.div
              initial={{ opacity: 0 }}
              animate={{ opacity: 1 }}
              exit={{ opacity: 0 }}
              onClick={() => setOpen(false)}
              style={{
                position: "fixed",
                inset: 0,
                background: "rgba(0,0,0,0.35)"
              }}
            />

            <motion.aside
              variants={menuVariants}
              initial="hidden"
              animate="show"
              exit="exit"
              style={{
                position: "fixed",
                top: 0,
                right: 0,
                width: 280,
                height: "100vh",
                background: "white",
                padding: 24,
                boxShadow: "-10px 0 30px rgba(0,0,0,0.1)"
              }}
            >
              <button onClick={() => setOpen(false)}>Close</button>
              <nav style={{ marginTop: 30, display: "grid", gap: 16 }}>
                <a href="#">Home</a>
                <a href="#">Services</a>
                <a href="#">Projects</a>
                <a href="#">Contact</a>
              </nav>
            </motion.aside>
          </>
        )}
      </AnimatePresence>
    </div>
  )
}
```

Why this is real-world useful:

* used in mobile responsive layouts
* better UX than instant open/close
* overlay and drawer feel polished

---

# 14) Real-world project 2: animated toast notification

```jsx
import { useState } from "react"
import { motion, AnimatePresence } from "motion/react"

export default function App() {
  const [toasts, setToasts] = useState([])

  function addToast() {
    const id = crypto.randomUUID()
    setToasts((prev) => [...prev, { id, text: "Item saved successfully" }])

    setTimeout(() => {
      setToasts((prev) => prev.filter((t) => t.id !== id))
    }, 2500)
  }

  return (
    <div style={{ padding: 40 }}>
      <button onClick={addToast}>Save</button>

      <div
        style={{
          position: "fixed",
          top: 20,
          right: 20,
          display: "grid",
          gap: 12
        }}
      >
        <AnimatePresence>
          {toasts.map((toast) => (
            <motion.div
              key={toast.id}
              initial={{ opacity: 0, x: 100, scale: 0.9 }}
              animate={{ opacity: 1, x: 0, scale: 1 }}
              exit={{ opacity: 0, x: 100, scale: 0.9 }}
              transition={{ type: "spring", stiffness: 180, damping: 18 }}
              style={{
                padding: "14px 18px",
                borderRadius: 14,
                background: "black",
                color: "white"
              }}
            >
              {toast.text}
            </motion.div>
          ))}
        </AnimatePresence>
      </div>
    </div>
  )
}
```

Used in:

* save notifications
* form success/error messages
* upload complete messages

---

# 15) Real-world project 3: reorderable todo list

Motion has official reorder components for React. ([Motion][8])

```jsx
import { useState } from "react"
import { Reorder } from "motion/react"

export default function App() {
  const [items, setItems] = useState([
    "Learn React",
    "Practice Motion",
    "Build Portfolio",
    "Apply for jobs"
  ])

  return (
    <div style={{ maxWidth: 400, margin: "40px auto" }}>
      <h2>Tasks</h2>

      <Reorder.Group
        axis="y"
        values={items}
        onReorder={setItems}
        style={{ display: "grid", gap: 12, padding: 0 }}
      >
        {items.map((item) => (
          <Reorder.Item
            key={item}
            value={item}
            style={{
              listStyle: "none",
              padding: 16,
              borderRadius: 14,
              background: "#f2f2f2",
              cursor: "grab"
            }}
          >
            {item}
          </Reorder.Item>
        ))}
      </Reorder.Group>
    </div>
  )
}
```

Perfect for:

* task apps
* playlist ordering
* kanban lanes
* dashboard widgets

---

# 16) Page transitions in React

Very common in portfolio sites and dashboards.

```jsx
import { motion, AnimatePresence } from "motion/react"
import { useState } from "react"

function Page({ text }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 40 }}
      animate={{ opacity: 1, y: 0 }}
      exit={{ opacity: 0, y: -40 }}
      transition={{ duration: 0.35 }}
      style={{
        marginTop: 30,
        padding: 30,
        borderRadius: 16,
        background: "#f5f5f5"
      }}
    >
      <h1>{text}</h1>
    </motion.div>
  )
}

export default function App() {
  const [page, setPage] = useState("Home")

  return (
    <div style={{ padding: 40 }}>
      <button onClick={() => setPage("Home")}>Home</button>
      <button onClick={() => setPage("About")}>About</button>

      <AnimatePresence mode="wait">
        <Page key={page} text={page} />
      </AnimatePresence>
    </div>
  )
}
```

This pattern is also used with React Router.

---

# 17) Motion values

Motion values are special animated values that update efficiently.

You’ll use them with hooks like:

* `useScroll`
* `useTransform`

They are helpful when you want animation state separate from React state.

Example concept:

```jsx
import { motion, useMotionValue } from "motion/react"

export default function App() {
  const x = useMotionValue(0)

  return (
    <>
      <button onClick={() => x.set(200)}>Move</button>
      <motion.div
        style={{
          x,
          width: 100,
          height: 100,
          background: "tomato",
          borderRadius: 12
        }}
      />
    </>
  )
}
```

---

# 18) Accessibility: reduced motion

Animations should not overwhelm users.

Motion provides support for reduced motion preferences, and its docs also discuss reduced-motion and bundle optimization strategies. ([Motion][9])

```jsx
import { motion, useReducedMotion } from "motion/react"

export default function App() {
  const shouldReduceMotion = useReducedMotion()

  return (
    <motion.div
      initial={{ opacity: 0, y: shouldReduceMotion ? 0 : 30 }}
      animate={{ opacity: 1, y: 0 }}
      transition={{ duration: 0.4 }}
    >
      Accessible content
    </motion.div>
  )
}
```

This is a good professional habit.

---

# 19) Performance tips

## Good practices

### 1. Animate `transform` and `opacity` most of the time

These are smoother than animating layout-heavy CSS properties.

Use:

* `x`, `y`
* `scale`
* `rotate`
* `opacity`

instead of constantly animating:

* `top`
* `left`
* `width`
* `height`

### 2. Use `layout` only when you need layout animation

Don’t add it everywhere.

### 3. Use variants for cleaner code

Especially in lists and sections.

### 4. Avoid excessive infinite animations

They can hurt UX and feel distracting.

### 5. Respect reduced motion

Better accessibility and usability.

### 6. Optimize bundle size when needed

Motion’s docs note that the standard `motion` component is heavier, while `LazyMotion` and smaller APIs like `useAnimate` can reduce the initial bundle substantially. ([Motion][9])

---

# 20) Common mistakes beginners make

## Mistake 1: forgetting keys in animated lists

```jsx
{items.map(item => (
  <motion.div key={item.id} />
))}
```

Without stable keys, animation can break.

---

## Mistake 2: wrapping `AnimatePresence` in the condition

Wrong:

```jsx
{open && (
  <AnimatePresence>
    <Modal />
  </AnimatePresence>
)}
```

Right:

```jsx
<AnimatePresence>
  {open && <Modal />}
</AnimatePresence>
```

That rule is documented by Motion. ([Motion][6])

---

## Mistake 3: over-animating everything

Too much animation makes UI feel slow and childish.

Use animation to:

* guide focus
* show state change
* improve feedback

Not just decoration.

---

## Mistake 4: using React state for every frame animation

For many advanced cases, Motion values are better.

---

# 21) A practical component structure for real apps

A clean pattern:

```jsx
// animations.js
export const fadeUp = {
  hidden: { opacity: 0, y: 30 },
  show: { opacity: 1, y: 0, transition: { duration: 0.5 } }
}

export const staggerContainer = {
  hidden: {},
  show: {
    transition: {
      staggerChildren: 0.12
    }
  }
}
```

Then use it anywhere:

```jsx
import { motion } from "motion/react"
import { fadeUp, staggerContainer } from "./animations"

export default function Features() {
  return (
    <motion.section
      variants={staggerContainer}
      initial="hidden"
      whileInView="show"
      viewport={{ once: true }}
    >
      <motion.h2 variants={fadeUp}>Features</motion.h2>
      <motion.p variants={fadeUp}>Fast and smooth UI animation.</motion.p>
    </motion.section>
  )
}
```

This makes big apps easier to manage.

---

# 22) Mini real-world landing page example

This combines hero entrance, staggered text, button hover, and scroll reveal.

```jsx
import { motion } from "motion/react"

const container = {
  hidden: {},
  show: {
    transition: { staggerChildren: 0.15 }
  }
}

const item = {
  hidden: { opacity: 0, y: 30 },
  show: { opacity: 1, y: 0, transition: { duration: 0.6 } }
}

function Hero() {
  return (
    <section
      style={{
        minHeight: "100vh",
        display: "grid",
        placeItems: "center",
        padding: 24
      }}
    >
      <motion.div
        variants={container}
        initial="hidden"
        animate="show"
        style={{ maxWidth: 700, textAlign: "center" }}
      >
        <motion.h1
          variants={item}
          style={{ fontSize: "clamp(2.5rem, 6vw, 5rem)", marginBottom: 16 }}
        >
          Build smoother React experiences
        </motion.h1>

        <motion.p
          variants={item}
          style={{ fontSize: "1.1rem", lineHeight: 1.6, marginBottom: 24 }}
        >
          Use Motion to create polished interfaces with better feedback,
          transitions, and interaction.
        </motion.p>

        <motion.div variants={item}>
          <motion.button
            whileHover={{ scale: 1.06 }}
            whileTap={{ scale: 0.97 }}
            style={{
              padding: "14px 22px",
              borderRadius: 14,
              border: "none",
              background: "black",
              color: "white",
              cursor: "pointer"
            }}
          >
            Get Started
          </motion.button>
        </motion.div>
      </motion.div>
    </section>
  )
}

function Feature({ title, text }) {
  return (
    <motion.div
      initial={{ opacity: 0, y: 40 }}
      whileInView={{ opacity: 1, y: 0 }}
      viewport={{ once: true, amount: 0.25 }}
      transition={{ duration: 0.5 }}
      style={{
        padding: 24,
        borderRadius: 20,
        background: "#f5f5f5"
      }}
    >
      <h3>{title}</h3>
      <p>{text}</p>
    </motion.div>
  )
}

export default function App() {
  return (
    <div>
      <Hero />

      <section
        style={{
          padding: 24,
          display: "grid",
          gridTemplateColumns: "repeat(auto-fit, minmax(240px, 1fr))",
          gap: 20
        }}
      >
        <Feature title="Fast feedback" text="Buttons, cards and forms feel alive." />
        <Feature title="Smooth transitions" text="Page and section changes feel natural." />
        <Feature title="Better UX" text="Animation helps users follow interface changes." />
      </section>
    </div>
  )
}
```

---

# 23) When to use Motion vs plain CSS

## Use CSS when:

* simple hover effect
* one small transition
* no React state-based mount/unmount animation needed

## Use Motion when:

* state-driven animation
* enter/exit animation
* gesture handling
* drag/reorder
* layout transitions
* scroll-linked animation
* complex orchestration

That is the main difference in practice.

---

# 24) Best learning path

Learn in this order:

1. install and `motion.div`
2. `initial`, `animate`, `transition`
3. `whileHover`, `whileTap`, `drag`
4. variants
5. `AnimatePresence`
6. `layout` and `layoutId`
7. `whileInView`
8. `useScroll` and `useTransform`
9. real projects

This order matches how Motion’s own React docs move from getting started into animation, gestures, layout, and more advanced APIs. ([Motion][2])

---

# 25) Final advice

For professional React projects, these are the most useful Motion features:

* `motion`
* `AnimatePresence`
* variants
* `whileHover` / `whileTap`
* `layout`
* `whileInView`
* `useScroll`
* `useTransform`

Master just these, and you can build most real-world UI animation.

If you want, next I’ll give you a **Framer Motion project pack with 10 real React components** like modal, sidebar, accordion, tabs, toast, pricing cards, hero section, image gallery, draggable card, and page transition.

[1]: https://motion.dev/?utm_source=chatgpt.com "Motion — JavaScript & React animation library"
[2]: https://motion.dev/docs/react?utm_source=chatgpt.com "Get started - React Animation Library - Motion.dev"
[3]: https://motion.dev/docs/react-installation?utm_source=chatgpt.com "How to install Motion for React"
[4]: https://motion.dev/docs/react-motion-component?utm_source=chatgpt.com "Motion component - React - Motion.dev"
[5]: https://motion.dev/docs/react-animation?utm_source=chatgpt.com "React Animation | Keyframes, Transitions & Gestures | Motion"
[6]: https://motion.dev/docs/react-animate-presence?utm_source=chatgpt.com "AnimatePresence — React exit animations - Motion.dev"
[7]: https://motion.dev/docs/react-use-transform?utm_source=chatgpt.com "useTransform — Composable React animation values - Motion"
[8]: https://motion.dev/docs/react-reorder?utm_source=chatgpt.com "Reorder — React drag-to-reorder animation - Motion.dev"
[9]: https://motion.dev/docs/react-reduce-bundle-size?utm_source=chatgpt.com "Reduce bundle size of Framer Motion - Motion.dev"
