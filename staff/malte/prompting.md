# Prompting THoughts & Footage Review

## Bear Benchmark

| #   | Style                  | Core idea                                                    | Rating                                        |
| --- | ---------------------- | ------------------------------------------------------------ | --------------------------------------------- |
| P01 | **Minimal/literal**    | Shortest possible correct description                        | 9 (lacks a bit of expression)                 |
| P02 | **Simple descriptive** | Hands-and-knees framing, no animal metaphor                  | 5 (only latter half)                          |
| P03 | **Action-sequence**    | Explicit step-by-step: bend → place hands → walk             | 5 (only latter half)                          |
| P04 | **Biomechanical**      | Joint-level language, "quadrupedal", "alternating limbs"     | 6                                             |
| P05 | **Character-driven**   | Explicitly frames it as acting/roleplay                      | 8 (one generation too fast)                   |
| P06 | **Theatrical/artsy**   | Poetic, evocative, surrender of posture                      | 5 (only latter half)                          |
| P07 | **Emotion-led**        | Mood descriptors: calm, heavy, unhurried                     | 2 (not moving like a bear, just getting down) |
| P08 | **Instructional**      | Stage/fitness-class direction style, colon-separated cues    | 0 (not on all fours)                          |
| P09 | **Metaphorical**       | Sensory detail: "knuckles grazing floor", "wild and ancient" | 7 (one generation failed)                     |
| P10 | **Verbose/cinematic**  | Full scene description with shoulder rolling etc.            | 10                                            |

<details>
  <summary>Prompts</summary>
  
    // P01
    "A person crawls on all fours like a bear",
    // P02
    "A person gets down on hands and knees and walks forward",
    // P03
    "A person bends down, places both hands on the ground, and walks on all fours with a heavy lumbering gait",
    // P04
    "A person shifts their weight forward onto extended arms, drops their knees to the ground, and moves in a quadrupedal walking pattern with alternating limb steps",
    // P05
    "A person plays the role of a bear, getting down on all fours and walking with slow powerful strides",
    // P06
    "A person inhabits the body of a great bear, surrendering upright posture, sinking hands to earth and prowling forward with primal deliberate weight",
    // P07
    "A person slowly and heavily crouches down onto all four limbs, moving with the calm unhurried confidence of a large animal",
    // P08
    "A person performs a bear walk: feet flat, hips high, arms straight, moving opposite hand and foot together across the ground",
    // P09
    "A person becomes four-legged, their spine horizontal, knuckles grazing the floor as they lumber forward like something wild and ancient",
    // P10
    "A person portraying a bear in a theatrical performance lowers themselves deliberately from a standing position onto both hands and feet, their torso parallel to the ground, then walks forward in a slow rhythmic quadrupedal motion, shoulders rolling with each step"
  
</details>

### Results

- direct, descriptive (and detailled) prompts work best
- poetic, metaphorical causes problems
- describing transitions does not work well
- - if a transition is needed, include it but ensure the final motion is described in equal or greater detail (see P10)
- model can not handle colon-separated list format well (P08)

## Verb Swap 

| Context             | Verbs tested                      | Rating     |
| ------------------- | --------------------------------- | ---------- |
| Quadrupedal         | `walks`, `crawls`, `lumbers`      | 10, 10, 10 |
| Upper-body gesture  | `reaches`, `extends`, `stretches` | 10, 7, 9   |
| Theatrical collapse | `collapses`, `sinks`              | 10, 5      |
| Performative walk   | `strides`, `walks`                | 7, 5       |


<details>
  <summary>Prompts</summary>
  
    // P01 — walks
    "A person walks on all fours, hands flat on the ground",
    // P02 — crawls
    "A person crawls on all fours, hands flat on the ground",
    // P03 — lumbers
    "A person lumbers on all fours, hands flat on the ground",
    // P04 — reaches
    "A person reaches both arms outward toward somebody in front of them",
    // P05 — extends
    "A person extends both arms outward toward somebody in front of them",
    // P06 — stretches
    "A person stretches both arms outward toward somebody in front of them",
    // P07 — collapses
    "A person collapses to the floor and lies still",
    // P08 — sinks
    "A person sinks to the floor and lies still",
    // P09 — strides
    "A person strides slowly across the stage with arms held out to the sides",
    // P10 — walks ctx2
    "A person walks slowly across the stage with arms held out to the sides"
  
</details>

### Results

- complex prompts: prefer the most common verb
- expressive verbs for simpler sentences

## Word Order

| Structure          | Pattern                                       | Rating |
| ------------------ | --------------------------------------------- | ------ |
| WO-A posture-first | *"A person, [posture], [verb]"*               | 5      |
| WO-B verb-first    | *"A person [verb], [posture as consequence]"* | 9      |
| WO-C end-placed    | *"A person [verb] until [final posture]"*     | 3      |

Applied consistently across quadrupedal locomotion (X), arm raise (Y), and kneeling bow (Z)

<details>
  <summary>Prompts</summary>
  
    // Context X — quadrupedal locomotion
    "A person, on all fours with hands flat on the ground, walks forward",          // X posture-first
    "A person walks forward, dropping onto all fours with hands flat on the ground",// X verb-first
    "A person walks forward and lowers into a quadrupedal position, hands flat on the ground", // X end-placed
    // Context Y — upper-body gesture
    "A person, standing upright with arms at their sides, slowly raises both arms above their head", // Y posture-first
    "A person slowly raises both arms above their head from a standing position",   // Y verb-first
    "A person stands upright and lifts both arms until fully raised above their head", // Y end-placed
    // Context Z — theatrical transition
    "A person, standing upright with arms at their sides, slowly kneels and bows their head", // Z posture-first
    "A person slowly kneels and bows their head from a standing position",          // Z verb-first
    "A person stands upright and lowers themselves until kneeling with head bowed"  // Z end-placed
  
</details>

The key hypothesis for B is whether Kimodo weights early tokens more heavily.

If WO-A consistently beats WO-C, that tells us to always anchor posture before the action verb.

### Results

! KIMODO DOES NOT WEIGH EARLY TOKENS MORE !

- verb placed first, directly followed by the posture works best
- - verb anchors the motion type
- - posture works best when it follows as a natural consequence of that verb
- describe one sustained motion with the verb, use all remaining words to describe the body's physical state during that exact moment