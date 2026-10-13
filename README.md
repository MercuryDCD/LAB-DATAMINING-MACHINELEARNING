# MLDM_Unibo\_public
Machine Learning and Data Mining course for University of Bologna

Prof. Claudio Sartori <claudio.sartori@unibo.it>

Tutor TBD

## Working with your own repository

This repository is meant to be kept in sync with the course materials while you work on your own copy.

- `origin` should point to your personal GitHub repository.
- `upstream` should point to the course repository.
- Use `git fetch upstream` followed by `git merge upstream/main` to bring in the weekly updates from the course.
- Push your own work with `git push origin main`.

Suggested setup after creating your personal repository:

```bash
git remote set-url origin https://github.com/<your-user>/<your-repo>.git
git remote -v
git push -u origin main
```

Weekly update flow:

```bash
git fetch upstream
git merge upstream/main
git push origin main
```

End-of-session checklist before leaving the lab:

```bash
git status
git add .
git commit -m "End of Lab xx"
git push origin main
```

If you have no changes to save, `git status` is enough.

###### Contains

 - lab activities
 - lab solutions 

###### Structure

The materials will be added and updated weekly according to scheduled labs, the solutions will be published six days after the lab.

The material on this repository reflects class activities during the last academic year. The material is not autonomous at all, and should be used only as a complement to class participation.

For students, this repository is a starting point. Keep your own notes, exercises, and notebook work in your personal repository, and pull the weekly course changes from `upstream`.

List of the scheduled labs

- Lab-topic-2-data-exploration
- Lab-topic-3a-class-dt-hold-out
- Lab-topic-3b-tuning-dt-grid-search-cv
- Lab-topic-3c-using-several-class
- Lab-topic-4-regression
- Lab-topic-5-clustering
- Lab-topic-6-attribute-selection
- Lab-topic-7-association-rules


