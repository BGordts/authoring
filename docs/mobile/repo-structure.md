# Repository Structure

A mobile course repository consists of a simple directory structure used by the
DataCamp build process to generate the views your students will be able to
interact with.

```text
|- assets/
|  |- images/
|  |- datasets
|- chapter1/
|  |- lesson1.yml
|  |- lesson2.yml
|  |- practice.yml
|- chapter2/
|  |- lesson1.yml
|  |- lesson2.yml
|  |- practice.yml
|- chapter3/
|  |- lesson1.yml
|  |- lesson2.yml
|  |- practice.yml
|- chapter4/
|  |- lesson1.yml
|  |- lesson2.yml
|  |- practice.yml
|- .gitignore
|- .circleci
|  |- config.yml
|- README.md
|- manifest.yml
```

| File/Folder                   | Description                                                                                                       |
| :--------------------         | :-------------------------------------------------------------------------                                        |
| `assets*/`                    | Folder containing all datasets or images you may want to use in your lessons (eg. csv / python/ R / image files). |
| `chapter*/`                   | Contains all the lessons for a particular chapter. This is a convention, not a strict requirement.                |
| `.gitignore`                  | Files and folders which should be ignored by Git.                                                                 |
| `README.md`                   | A readme file with a list of resources more explanation to get started.                                           |
| [`manifest.yml`](manifest.md) | A manifest file specifying metadata about the course, chapters, lessons, and assets.                              |

## Assets

This directory should contain all datasets or images you may want to use in your
lessons (eg. csv / python/ R / image files). You can organize it any way you
like, but sub-directories for images, datasets, etc. is one logical strategy.

## Chapters

Each chapter should belong to its own dedicated subdirectory. You can name
lessons according to  subject matter or ordinality at your own discretion.

## README

The course README should explain the objective of the course and outline the
curriculum at a high level.

## .gitignore

[Typical `.gitignore` file](https://www.atlassian.com/git/tutorials/gitignore).

## .circleci/config

The `.circleci/config.yml` file is the configuration for DataCamp's course build
process. It triggers the building and deployment of your course when you perform
certain actions, like merging into `development` / `master` branches, or tagging
a release. See [the development process documentation](development.md) for more
details.

## Manifest

The manifest file `manifest.yml` is what ties everything together. It lays out
the locations of your chapters, lessons, and assets. For example:

```yaml
authoringVersion: 1
title: Introduction to SQL
description: "Master the basics of querying databases with SQL, the world's most popular databasing language."
key: e738342c-15ca-4866-8786-500214d9a363
main_id: 1946
badge_url: https://s3.amazonaws.com/assets.datacamp.com/mobile/course_badges/SQL%402x.png
status: LIVE
technology_key: SQL

chapters:
  - key: 2caafbb1-c116-4c66-9c7f-857b1155ff3c
    title: Selecting Columns
    description: A brief introduction to working with relational databases.
    status: LIVE
    main_id: 5261
    lessons:
      - key: 5b0d45a2-6e81-44e5-b4a5-dc57ab3a50f9
        title: Beginning your SQL journey
        description: Learn the basics of SQL.
        practice: false
        fileName: chapter1/lesson1.yml
      - key: 5dbb1040-3ce4-40f6-a97c-f2eabf346309
        title: Learning to count
        description: Data science is mostly counting things.
        practice: false
        fileName: chapter1/lesson2.yml
      - key: eca415ab-fbd9-4f39-9930-724af8e10e36
        title: Practice
        description: "Practice what you've learned."
        practice: true
        fileName: chapter1/practice.yml

assets:
  - type: dataset
    key: people
    url: assets/datasets/people_small.csv
    tabs:
      table:
        title: People
  - type: dataset
    key: films
    url: assets/datasets/films_small.csv
    tabs:
      table:
        title: Films
  - type: dataset
    key: reviews
    url: assets/datasets/reviews.csv
    tabs:
      table:
        title: Reviews
  - type: dataset
    key: roles
    url: assets/datasets/roles.csv
    tabs:
      table:
        title: Roles
```

The [`mobile-teach` check](development.md) command will flag any mistakes made
in the manifest.

There are 4 key components to the manifest:

* Course metadata
* Chapter metadata
* Lesson metadata
* Asset metadata

Lets break down the example manifest above into these smaller chunks.

### Course metadata

```yaml
authoringVersion: 2
title: Introduction to SQL
description: "Master the basics of querying databases with SQL, the world's most popular databasing language."
key: e738342c-15ca-4866-8786-500214d9a363
# main_id: 1946
badge_url: https://s3.amazonaws.com/assets.datacamp.com/mobile/course_badges/SQL%402x.png
# status: LIVE
technology_key: SQL
```

* `authoringVersion` should always be 2 (version 1 of the authoring format is
  deprecated and only exists for the first 4 courses' backwards compatibility:
  Intro SQL, Intro R, Intro Python and Intermediate Python).
* `title` and `description` will be displayed at the course level in the app.
* `key` is the unique UUID for the course.
* If there is a corresponding desktop course, it is denoted with the `main_id`
  field.
* `badge_url` points to the course logo.
* `status` should be kept as `foo` until it is ready to be launched, at which
  point it will be set to `LIVE`.
* `technology_key`: Currently, one of `R`, `PYTHON`, or `SQL`.

### Chapter/lesson metadata

A chapter will not appear in your course unless it is specified here explicitly.

```yaml
chapters:
  - key: 2caafbb1-c116-4c66-9c7f-857b1155ff3c
    title: Selecting Columns
    description: A brief introduction to working with relational databases.
    status: LIVE
    main_id: 5261
    lessons:
      ...
```

* `key` is the unique UUID for the chapter.
* `title` and `description` will be displayed at the chapter level in the app.
* `status` should be kept as `foo` until it is ready to be launched, at which
  point it will be set to `LIVE`.
* If there is a corresponding desktop course, it is denoted with the `main_id`
  field.
* `lessons` is a list of all the lessons in the chapter and *their* associated
  metadata:

### Lessons metadata

Lesson metadata looks similar to course & chapter metadata.

```
lessons:
  - key: 5b0d45a2-6e81-44e5-b4a5-dc57ab3a50f9
    title: Beginning your SQL journey
    description: Learn the basics of SQL.
    practice: false
    fileName: chapter1/lesson1.yml
  - key: 5dbb1040-3ce4-40f6-a97c-f2eabf346309
    title: Learning to count
    description: Data science is mostly counting things.
    practice: false
    fileName: chapter1/lesson2.yml
  - key: eca415ab-fbd9-4f39-9930-724af8e10e36
    title: Practice
    description: "Practice what you've learned."
    practice: true
    fileName: chapter1/practice.yml
```

* `key` is the unique UUID for the chapter.
* `title` and `description` will be displayed at the chapter level in the app.
* `practice` is a Boolean: is this a practice lesson or not?
* `fileName` is the path to the lesson file from the root of the course
  repository.

### Assets metadata

Assets lay out the metadata for datasets that you might use to populate
[tabs](exercises/README.md) in your course. Any file can be rendered as a `file`
tab-type, while only CSV files can be rendered as a formatted `table` tab-type.

```yaml
assets:
  - type: dataset
    key: people
    url: assets/datasets/people_small.csv
    tabs:
      table:
        title: People
  - type: dataset
    key: films
    url: assets/datasets/films_small.csv
    tabs:
      file:
        title: films.csv
      table:
        title: Films
```

* `type`, specified as `dataset`.
* `key`, the alias for the asset.
* `url`, the path to the asset from the root of the course repository.
* `tabs`, with one entry for every possible tab-type, currently either `file` or
  `table`. Every tab-type requires the sub-field `title`, which will determine
  the name of the tab when the asset is rendered as the respective type. In the
  example above, the asset `films_small.csv` will have the title `films.csv`
  when rendered as a CSV file, and the title `Films` when rendered as a
  formatted table.