# Improve Resume

Turn the user's resume input into stronger software-development resume bullets. Put their best foot forward. Do not alter the truth or facts of the input. It is their experience. Do not lie.

## When to use

Use this workflow when Leonardo is asked to improve a resume, or when the app **Improve Resume** button is pressed.

## Hard constraints

- Keep the same employers, job titles, dates, teams, products, tools, and outcomes that appear in the input.
- Copy dates, titles, and numbers exactly. Use a hyphen in date ranges. Never use an en dash or em dash.
- Do not invent technologies, cloud platforms, languages, frameworks, certifications, awards, leadership, headcount, revenue, scale, or metrics.
- If a number is not in the input, do not add a number.
- Do not add implied impact such as efficiency, visibility, quality, scale, or lifecycle coverage unless the input states it.
- Do not add adjectives such as comprehensive, robust, full lifecycle, or enterprise-grade unless they appear in the input.
- Do not inflate seniority. Do not change "helped" into "led" unless the input already says they led.
- Do not drop facts that the user included. Same information, stronger wording.
- If a line is vague, tighten the language. Do not fill gaps with guesswork.

## What "best foot forward" means

Rewrite for what software hiring managers like to see:

- Start bullets with a strong past-tense action verb.
- Shape: action + what was built or improved + result only when the result is already in the input.
- Prefer concrete systems, products, and skills named in the input over soft filler.
- Use plain industry phrasing (APIs, tests, delivery, collaboration) only when it restates the input.
- Convert prose and duty lists into scannable bullets.
- Keep the original section order when sections exist (summary, experience, projects, skills, education).

## Output

Return the improved resume only. No preamble, no apology, no explanation of what you changed.

If the input is empty, do not invent a resume.
