# S3 Extract

Extract an archive file (zip file or tar file) stored on AWS S3. 

## Details

Downloads archive from S3 into memory, then extract and re-upload to given destination. 

The following S3 information is expected to be given as Environment Variables:

- `AWS_ENDPOINT_URL`
- `AWS_ACCESS_KEY_ID`
- `AWS_SECRET_ACCESS_KEY`
- `CERT_PATH` (optional)
- `CERT_DL_URL` (optional)
- `SIGNATURE_VERSION` (optional, defaults to "s3v4")
- `REGION_NAME` (optional, defaults to "us-east-1")

Additionally, these information needed for ClearML remote execution can also be given as Env Variable (optional, args will override env var):
- `DEFAULT_DOCKER_IMG`
- `DEFAULT_QUEUE`

For those not familiar, environment variables can be set through various ways, some being: 
- `export <NAME>=<VALUE>` in terminal
- can be set in `~/.bashrc` as well for more permanence

Iteratively extracting a folder of zips/tars is supported as well, through `--src-is-dir` flag.

## Usage

```
usage: run.py [-h] [--src-is-dir] [--dst-bucket DST_BUCKET] [--verbose] [--remote] [--clml-proj CLML_PROJ] [--clml-task-name CLML_TASK_NAME] [--clml-task-type CLML_TASK_TYPE]
              [--docker-img DOCKER_IMG] [--queue QUEUE]
              src_bucket src_path dst_path

positional arguments:
  src_bucket            Source bucket
  src_path              Source path
  dst_path              Destination path

optional arguments:
  -h, --help            show this help message and exit
  --src-is-dir          Flag to indicate that given src path is a directory. Will iteratively extract any files in it ending with .zip or .tar.
  --dst-bucket DST_BUCKET
                        Destination bucket (optional), will default to Source bucket.
  --verbose             print out current upload filename as it progresses
  --remote              use clearml to remotely run job
  --clml-proj CLML_PROJ
                        ClearML Project Name
  --clml-task-name CLML_TASK_NAME
                        ClearML Task Name
  --clml-task-type CLML_TASK_TYPE
                        ClearML Task Type, e.g. training, testing, inference, etc
  --docker-img DOCKER_IMG
                        Base docker image to pull for ClearML remote execution
  --queue QUEUE         ClearML remote execution queue
```

Example usage:

```bash
python run.py my-bucket dataset/coco/images.tar dataset/coco/ --verbose --remote --clml-proj coco --clml-task-name coco_extraction --docker-img ubuntu/20.04 --queue 1xGPU
```
