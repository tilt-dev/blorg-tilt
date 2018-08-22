def blorgly():
  return composite_service({"blorg_backend": blorg_backend(), "blorg_frontend": blorg_frontend()})

def blorg_backend():
  entrypoint = '/go/bin/blorg-backend'
  image = build_docker_image('Dockerfile.base', 'blorg.io/blorgdev/blorg-backend', entrypoint)
  repo = local_git_repo('../blorg-backend')
  src_dir = '/go/src/github.com/windmilleng/blorg-backend'
  image.add(src_dir, repo)
  image.run("go install github.com/windmilleng/blorg-backend/...")
  yaml = read_file('blorg_backend.yaml')
  return k8s_service(yaml, image)

def blorg_frontend():
  entrypoint = '/go/bin/blorg-frontend --backendAddr lb-blorg-be:8080'
  image = build_docker_image('Dockerfile.base', 'blorg.io/blorgdev/blorg-frontend', entrypoint)
  repo = local_git_repo('../blorg-frontend')
  src_dir = '/go/src/github.com/windmilleng/blorg-frontend'
  image.add(src_dir, repo)
  image.run("go install github.com/windmilleng/blorg-frontend/...")
  yaml = read_file('blorg_frontend.yaml')
  return k8s_service(yaml, image)

def read_file(filepath):
  return local("cat " + filepath)
