---
name: banana-angle-markdown
description: Use when generating camera angles for BANANA sequences without code dependencies - provides structured markdown tables for scoring and selecting cinematographic angles based on scene requirements, director styles, and narrative functions through systematic evaluation (project, gitignored)
---

# BANANA Camera Angle Selection (Markdown-Based)

## Overview
Intelligent camera angle selection for BANANA image generation using structured markdown evaluation instead of code. Scores angles across 7 dimensions and generates operations.json through systematic markdown-based reasoning.

## When to Use
- User requests camera angles for narrative scenes
- Need to generate operations.json for BANANA
- Want transparent scoring visible to users
- Working without Python dependencies
- Need director-style cinematography (Fincher, Ray, Anderson, Villeneuve, Nolan)

---

## MANDATORY WORKFLOW: Image Analysis → Questions → Generation

### Phase 0: Analyze Input Image

**BEFORE asking any questions, you MUST:**

1. Use `Read` tool to view the input image
2. Extract and report:
   - Character description (clothing, features, pose)
   - Environment/setting detected
   - Lighting style observed
   - Art style (realistic, anime, illustrated, etc.)
   - Mood/atmosphere conveyed

**Example Analysis Output:**
```
📷 IMAGE ANALYSIS COMPLETE
━━━━━━━━━━━━━━━━━━━━━━━━━━
Character: Astronaut in orange-visored space suit
Setting: Spacecraft interior with overhead lights
Lighting: Cool blue ambient with warm orange accents
Style: Anime/illustrated, cinematic
Mood: Contemplative, isolated
```

### Phase 1: Ask User Questions (3 Rounds)

Use `AskUserQuestion` tool to gather parameters. Max 4 questions per round.

---

#### ROUND 1: Setup & Technical (4 questions)

```json
{
  "questions": [
    {
      "header": "Beats",
      "question": "How many story beats in this sequence?",
      "options": [
        {"label": "3 beats", "description": "Compact sequence (7 shots)"},
        {"label": "4 beats", "description": "Standard sequence (10 shots)"},
        {"label": "5 beats", "description": "Extended sequence (12 shots)"},
        {"label": "7 beats", "description": "Epic sequence (17 shots)"}
      ],
      "multiSelect": false
    },
    {
      "header": "Shots",
      "question": "How many total shots?",
      "options": [
        {"label": "Auto-calculate", "description": "2-3 shots per beat automatically"},
        {"label": "Minimal", "description": "1 shot per beat"},
        {"label": "Dense", "description": "3-4 shots per beat"},
        {"label": "Fixed count", "description": "Specify exact number"}
      ],
      "multiSelect": false
    },
    {
      "header": "Director",
      "question": "Which cinematographic style?",
      "options": [
        {"label": "Villeneuve", "description": "Epic, atmospheric, wide shots"},
        {"label": "Fincher", "description": "Precise, psychological, dutch angles"},
        {"label": "Ray", "description": "Humanistic, natural, eye-level"},
        {"label": "Nolan", "description": "IMAX, practical, structural"}
      ],
      "multiSelect": false
    },
    {
      "header": "Lighting",
      "question": "Lighting treatment?",
      "options": [
        {"label": "Match image", "description": "Use detected lighting from input"},
        {"label": "High-key", "description": "Bright, even, optimistic"},
        {"label": "Low-key", "description": "Shadows, contrast, dramatic"},
        {"label": "Stylized", "description": "Neon, color gel, expressive"}
      ],
      "multiSelect": false
    }
  ]
}
```

---

#### ROUND 2: Story & World (4 questions)

```json
{
  "questions": [
    {
      "header": "Story Input",
      "question": "How will you provide the narrative?",
      "options": [
        {"label": "Beat list", "description": "e.g., 'Awakening, Discovery, Escape'"},
        {"label": "Synopsis", "description": "1-2 sentence story summary"},
        {"label": "Script excerpt", "description": "Detailed scene description"},
        {"label": "Just an idea", "description": "Concept only, I'll expand it"}
      ],
      "multiSelect": false
    },
    {
      "header": "Arc Type",
      "question": "What narrative arc?",
      "options": [
        {"label": "Discovery", "description": "Find something new"},
        {"label": "Survival", "description": "Overcome threat/danger"},
        {"label": "Transformation", "description": "Internal/emotional change"},
        {"label": "Mission", "description": "Complete an objective"}
      ],
      "multiSelect": false
    },
    {
      "header": "Setting",
      "question": "Confirm or change detected setting: [DETECTED_SETTING]",
      "options": [
        {"label": "Confirm", "description": "Use the detected setting"},
        {"label": "Interior", "description": "Different interior location"},
        {"label": "Exterior", "description": "Outdoor/outside environment"},
        {"label": "Multiple", "description": "Character moves between locations"}
      ],
      "multiSelect": false
    },
    {
      "header": "Time Scope",
      "question": "How much time passes?",
      "options": [
        {"label": "Minutes", "description": "Real-time, single scene"},
        {"label": "Hours", "description": "One event/shift"},
        {"label": "Days", "description": "Extended journey"},
        {"label": "Abstract", "description": "Dreamlike, time unclear"}
      ],
      "multiSelect": false
    }
  ]
}
```

---

#### ROUND 3: Character Journey (4 questions)

```json
{
  "questions": [
    {
      "header": "Start State",
      "question": "Character's situation at start?",
      "options": [
        {"label": "At rest", "description": "Sleeping, waiting, idle"},
        {"label": "Preparing", "description": "Suiting up, planning"},
        {"label": "Mid-action", "description": "Working, traveling"},
        {"label": "In crisis", "description": "Danger, emergency"}
      ],
      "multiSelect": false
    },
    {
      "header": "Movement",
      "question": "How far does character move?",
      "options": [
        {"label": "Static", "description": "Stays in same spot"},
        {"label": "Same area", "description": "Moves within room/space"},
        {"label": "Exits", "description": "Leaves to new environment"},
        {"label": "Multiple", "description": "Several location changes"}
      ],
      "multiSelect": false
    },
    {
      "header": "Tone",
      "question": "Dominant emotional quality?",
      "options": [
        {"label": "Wonder/Awe", "description": "Amazement, discovery"},
        {"label": "Tension/Dread", "description": "Fear, suspense"},
        {"label": "Contemplative", "description": "Thoughtful, melancholy"},
        {"label": "Triumphant", "description": "Hopeful, victorious"}
      ],
      "multiSelect": false
    },
    {
      "header": "Climax",
      "question": "What happens at peak moment?",
      "options": [
        {"label": "Discovery", "description": "Finds object/place"},
        {"label": "Realization", "description": "Understands something"},
        {"label": "Action", "description": "Confrontation/escape"},
        {"label": "Transcendence", "description": "Transformation/merge"}
      ],
      "multiSelect": false
    }
  ]
}
```

---

### Phase 2: Collect Story Beats

After Round 2, if user selected "Beat list" or "Synopsis", prompt them:

> **Please provide your story beats or synopsis now.**
>
> Examples:
> - Beat list: "Awakening, EVA Departure, Discovery, Transcendence"
> - Synopsis: "An astronaut discovers an alien artifact in deep space and merges with it"

### Phase 3: Generate Sequence

With all answers collected, proceed to scoring and operations.json generation using the rubrics below.

---

## Step 1: Requirements Gathering

Fill in the scene requirements based on user answers:

### Scene Analysis Checklist
- [ ] **Scene Type**: ___________ (dialogue/action/emotional/establishing/montage)
- [ ] **Character Count**: ___________ (solo/two_person/group)
- [ ] **Mood Tags**: ___________ (select from: tense, mysterious, intimate, urgent, vulnerable, powerful, contemplative)
- [ ] **Narrative Function**: ___________ (introduction/buildup/climax/revelation/resolution)
- [ ] **Director Style**: ___________ (fincher/satyajit_ray/wes_anderson/villeneuve/nolan/none)
- [ ] **Number of Beats**: ___________ (for sequence generation)
- [ ] **Input Image**: ___________ (character/base image path)

### Scene Type Detection Guide

| User Keywords | Scene Type | Typical Angles |
|---------------|------------|----------------|
| talk, conversation, discuss, meeting | dialogue | OTS, two-shot, close-ups, medium eye-level |
| chase, run, fight, escape, action | action | tracking, POV, dynamic, wide tracking |
| cry, grief, love, loss, emotional | emotional | ECU, intimate, static, close-ups |
| establish, location, where, setting | establishing | wide, aerial, environmental, high angle |
| sequence, montage, progression | montage | varied, rhythmic, progressive |

## Step 2: Scoring System

### Multi-Dimensional Scoring Rubric

Evaluate each angle against these dimensions:

| Dimension | Max Points | Evaluation Criteria |
|-----------|------------|---------------------|
| **Scene Match** | 20 | Does angle suit the scene type? |
| **Character Framing** | 15 | Appropriate for character count? |
| **Mood Alignment** | 25 | Matches emotional tone? |
| **Director Style** | 30 | Fits director preference? |
| **Narrative Function** | 10 | Serves story beat purpose? |
| **Technical Feasibility** | 10 | Static vs dynamic capability? |
| **Creative Bonus** | 15 | Unique/memorable composition? |
| **TOTAL** | 125 | Angles scoring >60 are viable |

### Director Style Preference Matrix

| Angle Type | Fincher | Ray | Anderson | Villeneuve | Nolan |
|------------|---------|-----|----------|------------|-------|
| High Angle | 8/10 | 4/10 | 6/10 | 7/10 | 5/10 |
| Low Angle | 6/10 | 3/10 | 4/10 | 8/10 | 6/10 |
| Dutch Angle | 10/10 | 2/10 | 3/10 | 4/10 | 7/10 |
| Eye Level | 5/10 | 10/10 | 7/10 | 6/10 | 8/10 |
| ECU | 9/10 | 3/10 | 2/10 | 6/10 | 4/10 |
| Wide Shot | 7/10 | 8/10 | 9/10 | 10/10 | 9/10 |
| OTS | 8/10 | 7/10 | 5/10 | 6/10 | 7/10 |
| Tracking | 7/10 | 5/10 | 4/10 | 8/10 | 9/10 |
| POV | 6/10 | 3/10 | 1/10 | 7/10 | 8/10 |
| Overhead | 9/10 | 4/10 | 10/10 | 8/10 | 6/10 |

## Step 3: Core Camera Angle Reference

### Essential Angles Database

#### 1. High Angle Shot
- **ID**: `high_angle_shot`
- **Prompt**: "Give me a high angle shot of this image"
- **Scene Types**: ✓ dialogue, ✓ action, ✓ emotional, ✓ establishing
- **Character Count**: ✓ solo, ✓ two_person, ✓ group
- **Mood Match**: vulnerable, observational, contemplative, weak
- **Narrative Function**: introduction, buildup, revelation
- **Director Scores**: Fincher(8), Ray(4), Anderson(6), Villeneuve(7), Nolan(5)

#### 2. Low Angle (Power)
- **ID**: `low_angle_power`
- **Prompt**: "Give me a low angle shot showing power and dominance"
- **Scene Types**: ✓ dialogue, ✓ action, emotional, establishing
- **Character Count**: ✓ solo, ✓ two_person, group
- **Mood Match**: powerful, dominant, heroic, intimidating
- **Narrative Function**: introduction, climax, revelation
- **Director Scores**: Fincher(6), Ray(3), Anderson(4), Villeneuve(8), Nolan(6)

#### 3. Dutch Angle (Tilted)
- **ID**: `dutch_angle_unease`
- **Prompt**: "Give me a dutch angle tilted shot creating unease"
- **Scene Types**: ✓ dialogue, ✓ action, ✓ emotional, montage
- **Character Count**: ✓ solo, ✓ two_person, ✓ group
- **Mood Match**: tense, mysterious, disorienting, unstable
- **Narrative Function**: buildup, climax, revelation
- **Director Scores**: Fincher(10), Ray(2), Anderson(3), Villeneuve(4), Nolan(7)

#### 4. Eye Level Medium
- **ID**: `eye_level_medium`
- **Prompt**: "Give me an eye level medium shot"
- **Scene Types**: ✓ dialogue, action, ✓ emotional, ✓ establishing
- **Character Count**: ✓ solo, ✓ two_person, group
- **Mood Match**: neutral, respectful, observational, natural
- **Narrative Function**: ✓ all functions
- **Director Scores**: Fincher(5), Ray(10), Anderson(7), Villeneuve(6), Nolan(8)

#### 5. Extreme Close-Up (ECU)
- **ID**: `extreme_close_up`
- **Prompt**: "Give me an extreme close-up focusing on eyes or details"
- **Scene Types**: ✓ dialogue, action, ✓ emotional, montage
- **Character Count**: ✓ solo, two_person, group
- **Mood Match**: intimate, tense, vulnerable, psychological
- **Narrative Function**: buildup, climax, revelation
- **Director Scores**: Fincher(9), Ray(3), Anderson(2), Villeneuve(6), Nolan(4)

#### 6. Wide Establishing Shot
- **ID**: `wide_establishing`
- **Prompt**: "Give me a wide establishing shot showing environment"
- **Scene Types**: dialogue, action, emotional, ✓ establishing
- **Character Count**: solo, two_person, ✓ group
- **Mood Match**: epic, isolated, contextual, grand
- **Narrative Function**: ✓ introduction, buildup, resolution
- **Director Scores**: Fincher(7), Ray(8), Anderson(9), Villeneuve(10), Nolan(9)

#### 7. Over-Shoulder (OTS)
- **ID**: `over_shoulder`
- **Prompt**: "Give me an over-the-shoulder shot"
- **Scene Types**: ✓ dialogue, action, ✓ emotional, montage
- **Character Count**: solo, ✓ two_person, group
- **Mood Match**: confrontational, intimate, connected, tense
- **Narrative Function**: ✓ all functions
- **Director Scores**: Fincher(8), Ray(7), Anderson(5), Villeneuve(6), Nolan(7)

#### 8. POV (Point of View)
- **ID**: `pov_first_person`
- **Prompt**: "Give me a first-person POV shot"
- **Scene Types**: dialogue, ✓ action, ✓ emotional, montage
- **Character Count**: ✓ solo, two_person, group
- **Mood Match**: immersive, urgent, subjective, panic
- **Narrative Function**: buildup, ✓ climax, revelation
- **Director Scores**: Fincher(6), Ray(3), Anderson(1), Villeneuve(7), Nolan(8)

#### 9. Tracking Shot
- **ID**: `tracking_lateral`
- **Prompt**: "Give me a smooth lateral tracking shot"
- **Scene Types**: dialogue, ✓ action, emotional, ✓ establishing
- **Character Count**: ✓ solo, ✓ two_person, group
- **Mood Match**: dynamic, urgent, following, smooth
- **Narrative Function**: ✓ all functions
- **Director Scores**: Fincher(7), Ray(5), Anderson(4), Villeneuve(8), Nolan(9)

#### 10. Overhead/Bird's Eye
- **ID**: `overhead_birds_eye`
- **Prompt**: "Give me an overhead bird's eye view"
- **Scene Types**: dialogue, ✓ action, emotional, ✓ establishing
- **Character Count**: solo, two_person, ✓ group
- **Mood Match**: omniscient, isolated, pattern, geometric
- **Narrative Function**: introduction, revelation, resolution
- **Director Scores**: Fincher(9), Ray(4), Anderson(10), Villeneuve(8), Nolan(6)

### Specialized Angle Categories

#### Action Sequences (35 angles)
- Wide Tracking Run - urgent chase, tracking movement
- POV Chase - immersive panic, handheld
- Crash Zoom Impact - sudden focus, dramatic
- Whip Pan Transition - fast movement, energy
- Low Angle Athletic - power in motion

#### Intimate Dialogue (25 angles)
- Close-Up Reaction - emotional response
- Two-Shot Balance - equal perspective
- Profile Confrontation - side view tension
- Slow Push-In - building intimacy
- Mirror Reflection - duality, introspection

#### Emotional Beats (20 angles)
- ECU Tears - vulnerability, grief
- Isolated Wide - loneliness, contemplation
- Handheld Anxiety - nervous energy
- Static Contemplation - quiet reflection
- Window Light Portrait - hope, transition

#### Environmental Storytelling (15 angles)
- Aerial Establishing - scope, geography
- Through-Object Frame - voyeuristic, hidden
- Negative Space - isolation, minimalism
- Deep Focus Layer - complex staging
- Architectural Frame - using environment

## Step 4: Multi-Angle Coverage Rules

### Beat Distribution Table

For sequences with multiple beats, use this distribution:

| Beat Type | Shot Count | Framing Sequence | Priority |
|-----------|------------|------------------|----------|
| **First Beat** | 3 shots | Wide → Medium → Close | Critical |
| **Standard Beat** | 2 shots | Wide → Medium/Close | Normal |
| **Climax Beat** | 3 shots | Wide → Medium → Close | Critical |
| **Last Beat** | 3 shots | Wide → Medium → Close | Critical |

### Auto-Distribution Formula
- 5 beats = 12 shots total (3+2+2+2+3)
- 3 beats = 7 shots total (3+2+2)
- 7 beats = 17 shots total (3+2+2+3+2+2+3)

## Step 5: Angle Selection Process

### Manual Scoring Workflow

For each angle in the database:

1. **Scene Match** (0-20 points)
   - Perfect match = 20
   - Good match = 15
   - Possible = 10
   - Poor fit = 0

2. **Character Framing** (0-15 points)
   - Designed for this count = 15
   - Works well = 10
   - Possible = 5
   - Inappropriate = 0

3. **Mood Alignment** (0-25 points)
   - Multiple mood matches = 25
   - Single mood match = 15
   - Neutral = 10
   - Contradicts mood = 0

4. **Director Style** (0-30 points)
   - Multiply director matrix score × 3
   - Example: Fincher + Dutch = 10/10 × 3 = 30 points

5. **Sum Total Score**
   - Scores >60 = Good selection
   - Scores >80 = Excellent selection
   - Scores <40 = Consider creative alternatives

### Creative Angle Generation

When catalog scores are low (<60 average), generate creative angles:

#### Creative Pattern Templates

**Reflection Pattern**
- Formula: [BASE ANGLE] + "reflected in [SURFACE]"
- Example: "Low angle reflected in rain puddle"
- Mood Boost: +mysterious, +psychological

**Through-Object Pattern**
- Formula: [BASE ANGLE] + "through [OBSTRUCTION]"
- Example: "Medium shot through venetian blinds"
- Mood Boost: +voyeuristic, +tense

**Split Diopter Pattern**
- Formula: [ANGLE 1] + [ANGLE 2] + "split focus"
- Example: "Close-up foreground, medium background split focus"
- Mood Boost: +complex, +layered

**Motion Blur Pattern**
- Formula: [BASE ANGLE] + "with [MOTION TYPE] blur"
- Example: "Static medium with passing motion blur"
- Mood Boost: +dynamic, +temporal

## Step 6: Operations.json Generation

### Template Structure

```json
{
  "sequence_metadata": {
    "title": "[THEME DESCRIPTION]",
    "theme": "[THEME]",
    "total_shots": [NUMBER],
    "director_style": "[STYLE OR null]",
    "lighting_consistency": "[LIGHTING SPEC]",
    "style_consistency": "[STYLE SPEC]",
    "generated_at": "[TIMESTAMP]"
  },
  "operations": [
    {
      "prompt": "[GENERATED PROMPT - SEE TEMPLATE BELOW]",
      "input_images": ["[INPUT_IMAGE_PATH]"],
      "output_name": "shot_[NUMBER]_[BEAT_DESCRIPTION].png",
      "continuity_ids": ["sequence"],
      "metadata": {
        "shot_number": [NUMBER],
        "beat": "[BEAT DESCRIPTION]",
        "beat_index": [INDEX],
        "angle_id": "[ANGLE_ID]",
        "angle_name": "[ANGLE_NAME]",
        "angle_score": [SCORE],
        "rationale": ["[REASON1]", "[REASON2]", "[REASON3]"]
      }
    }
  ]
}
```

### Prompt Construction Template

Build prompts by combining these elements in order:

1. **Character Preservation**: `PRESERVE CHARACTER EXACTLY: Character from input image.`
2. **Scene Change**: `SCENE CHANGE: [BEAT DESCRIPTION].`
3. **Style Preservation**: `PRESERVE STYLE: [STYLE_CONSISTENCY].`
4. **Camera**: `CAMERA: [ANGLE_NAME], [static/tracking/handheld] camera.`
5. **Composition**: `COMPOSITION: [wide/medium/close] framing, [rule of thirds/centered] composition.`
6. **Lighting**: `LIGHTING: [LIGHTING_CONSISTENCY OR DEFAULT].`
7. **Technical**: `TECHNICAL: 16:9 cinematic, high-quality.`

### Example Prompt Assembly

```
PRESERVE CHARACTER EXACTLY: Character from input image.
SCENE CHANGE: Character watches target from dark alley.
PRESERVE STYLE: Film noir aesthetic with modern precision.
CAMERA: High Angle Shot, static camera.
COMPOSITION: Medium framing, rule of thirds composition.
LIGHTING: Low-key noir lighting, hard shadows, single source.
TECHNICAL: 16:9 cinematic, high-quality.
```

## Step 7: Validation Checklist

Before finalizing operations.json:

### Technical Validation
- [ ] All shots have unique output names
- [ ] Input image path is consistent
- [ ] Continuity IDs match across sequence
- [ ] Shot numbers are sequential
- [ ] Beat indices are correct

### Creative Validation
- [ ] Angles provide coverage variety (wide/medium/close)
- [ ] Director style is consistently applied
- [ ] Mood is maintained across sequence
- [ ] No duplicate angles in same beat
- [ ] Creative angles used when appropriate

### Prompt Validation
- [ ] Character preservation statement present
- [ ] Scene change clearly described
- [ ] Camera angle explicitly stated
- [ ] Composition details included
- [ ] Lighting consistency maintained

## Example Workflow

### User Input
"Generate camera angles for a tense interrogation scene between two people, Fincher style"

### Step 1: Requirements
- Scene Type: dialogue
- Character Count: two_person
- Mood Tags: tense, confrontational
- Narrative Function: climax
- Director Style: fincher

### Step 2: Score Top Angles

**Dutch Angle**:
- Scene Match: 20 (perfect for dialogue)
- Character: 15 (designed for two_person)
- Mood: 25 (matches tense)
- Director: 30 (Fincher 10/10)
- Function: 10 (good for climax)
- **Total: 100**

**Over-Shoulder**:
- Scene Match: 20
- Character: 15
- Mood: 20 (confrontational)
- Director: 24 (Fincher 8/10)
- Function: 10
- **Total: 89**

**ECU**:
- Scene Match: 20
- Character: 10
- Mood: 25 (psychological)
- Director: 27 (Fincher 9/10)
- Function: 10
- **Total: 92**

### Step 3: Generate Output

Select top 3 angles and build operations.json with proper prompts.

## Quick Reference Tables

### Mood to Angle Mapping

| Mood | Best Angles |
|------|-------------|
| Tense | Dutch, ECU, OTS, Handheld |
| Vulnerable | High Angle, ECU, Isolated Wide |
| Powerful | Low Angle, Wide Hero, Centered |
| Intimate | Close-Up, Soft Focus, Eye Level |
| Mysterious | Silhouette, Through-Object, Reflection |
| Urgent | Tracking, POV, Whip Pan, Handheld |

### Scene Type Defaults

| Scene Type | Primary Angles | Avoid |
|------------|---------------|-------|
| Dialogue | OTS, Two-Shot, Close-Ups | Extreme Wide, Aerial |
| Action | Tracking, POV, Wide, Dynamic | Static ECU, Contemplative |
| Emotional | ECU, Close-Up, Intimate | Wide, Distant, Cold |
| Establishing | Wide, Aerial, Environmental | ECU, Close-Ups |

### Director Quick Guide

| Director | Signature Angles | Avoid |
|----------|------------------|-------|
| Fincher | Dutch, ECU, Overhead, Precise | Handheld, Whimsical |
| Ray | Eye Level, Natural, Medium | Dutch, Extreme Angles |
| Anderson | Centered, Symmetrical, Flat | Handheld, Dutch |
| Villeneuve | Wide, Epic, Environmental | Rapid Cuts, ECU |
| Nolan | IMAX Wide, Practical, Dutch | Whimsical, Soft |

## Common Mistakes

1. **Over-scoring mood match** - Don't give 25 points for vague alignment
2. **Ignoring director style** - This is worth 30 points, use it
3. **Duplicate angles** - Don't use same angle twice in one beat
4. **Missing coverage** - Ensure mix of wide/medium/close
5. **Prompt inconsistency** - Keep lighting/style specs identical

## Performance Tracking

Track successful angles for future reference:

### Success Log Template

| Angle | Usage Count | Success Rate | Best For | Avoid When |
|-------|-------------|--------------|----------|------------|
| High Angle | [COUNT] | [%] | Vulnerable moments | Power scenes |
| Dutch Angle | [COUNT] | [%] | Psychological tension | Ray style |
| ECU | [COUNT] | [%] | Emotional intimacy | Establishing |

Update after each use to improve selection accuracy.

## Related Resources

- Original Python implementation: `/Users/mac/AI/claudeOS/skills/banana_angle_selection/`
- BANANA image generation workflow documentation
- Director style analysis guides
- Cinematography reference books

---

**Note**: This markdown-based approach provides transparent scoring and selection without code dependencies. All calculations are performed through systematic evaluation using the tables and rubrics above.