# workshop-r [![gh-actions-build-status](https://github.com/nbisweden/workshop-r/workflows/build/badge.svg)](https://github.com/nbisweden/workshop-r/actions?workflow=build)

This repo contains the course material for NBIS workshop **R Programming Foundation for Life Scientists**. The rendered view of this repo is available [here](https://nbisweden.github.io/workshop-r/).

## Contributing

To add or update contents of this repo (for collaborators), first clone the repo.

```
git clone https://github.com/nbisweden/workshop-r.git
```

Make changes/updates as needed. Add the changed files. Commit it. Then push the repo back.

```
git add .
git commit -m "I did this and that"
git push origin
```

If you are not added as a collaborator, first fork this repo to your account, then clone it locally, make changes, commit, push to your repo, then submit a pull request to this repo.

:exclamation: When updating repo for a new course, change `output_dir: XXXX` in `_site.yml` as the first thing, so that old rendered files are not overwritten.

:exclamation: Do not push any rendered .html files or intermediates.

## Repo organisation

The source material is located on the *master* branch (default). The rendered material is located on the *gh-pages* branch. For most part, one only needs to update content in master. Changes pushed to the *master* branch is automatically rendered to the *gh-pages* branch.

For more details about repo organisation, updating and modifying this repo, check out the [template repo](https://github.com/royfrancis/workshop-template-rmd-ga).

### Schedule

Schedule is saved into the `schedule.csv` file. It is a csv file with semi-colon as delimiter. You should NOT try to edit it in a text editor, but use proper spreadsheet.
If you are using command line, you can install `vd` and open and edit the file `vd --csv-delimiter ';' schedule.csv`.

### Docker

R packages needed to build the website and run the labs are all contained in a Docker container. To run docker container locally, follow instructions below:

:exclamation: Image is about 4.8 GB!

```
# pull the container
docker pull --platform=linux/amd64 ghcr.io/nbisweden/workshop-r:latest

# render whole website
docker run --platform=linux/amd64 --rm -u $(id -u ${USER}):$(id -g ${USER}) -v ${PWD}:/rmd ghcr.io/nbisweden/workshop-r:latest Rscript -e 'rmarkdown::render_site()'

# render one file
docker run --platform=linux/amd64 --rm -u $(id -u ${USER}):$(id -g ${USER}) -v ${PWD}:/rmd ghcr.io/nbisweden/workshop-r:latest Rscript -e 'rmarkdown::render("index.Rmd")'
```

To run RStudio server and develop in the browser, run;

```
docker run --platform=linux/amd64 --rm -e PASSWORD=rstudio -p 8788:8787 -v ${PWD}:/rmd ghcr.io/nbisweden/workshop-r:latest
```

Go to [http://localhost:8788/](http://localhost:8788/) or [http://0.0.0.0:8788](http://0.0.0.0:8788). Username is `rstudio` and password is `rstudio`. Change to folder `/rmd` to see your files.

To add new packages, you need to update `Dockerfile`, rebuild the container, test it and push it to repository. Make changes to the `Dockerfile` as needed. Then to rebuild and push the docker image, follow the steps below:

:exclamation: Remember to update the version number
:exclamation: Remember to render the whole website to make sure everything works

```
# build container and add tags
docker build --platform=linux/amd64 -t ghcr.io/nbisweden/workshop-r:1.1.2 .
docker tag ghcr.io/nbisweden/workshop-r:1.1.2 ghcr.io/nbisweden/workshop-r:latest

# push to ghcr
docker login ghcr.io
docker push ghcr.io/nbisweden/workshop-r:1.1.2
docker push ghcr.io/nbisweden/workshop-r:latest
```

---

**2024** NBIS • SciLifeLab
