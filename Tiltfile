def blorg_backend():
  entrypoint = '/app/server'
  image = build_docker_image('Dockerfile.base', 'gcr.io/blorg-dev/blorg-backend:' + '-' + local('whoami').rstrip('\n'), entrypoint)
  src_dir = '/go/src/github.com/windmilleng/blorg-backend'
  image.add_mount(src_dir, git_repo('.'))
  image.add_cmd('cd ' + src_dir + '; go get ./...')
  image.add_cmd('mkdir -p /app')
  image.add_cmd('cd ' + src_dir + '; go build -o server; cp server /app/')
  # print(image)
  yaml = local('python ~' + src_dir + '/populate_config_template.py ' + ' 1>&2 && cat k8s-conf.generated.yaml')

  # print(yaml)
  return k8s_service(yaml, image)

def blorgly_backend():
  entrypoint = '/app/server'
  src_dir = '/go/src/github.com/windmilleng/blorgly-backend'
  image = build_docker_image('../blorgly-backend/Dockerfile.base', 'gcr.io/blorg-dev/blorgly-backend:' + '-' + local('whoami').rstrip('\n'), entrypoint)
  image.add_mount(src_dir, git_repo('../blorgly-backend'))
  image.add_cmd('cd ~' + src_dir + '; go get ./...')
  image.add_cmd('mkdir -p /app')
  image.add_cmd('cd ' + src_dir + '; go build -o server; cp server /app/')
  # print(image)
  # populate = local('python ~' + src_dir + '/populate_config_template.py ' + env + ' 1>&2')
  # generated = local('cat ../blorgly-backend/k8s-conf.generated.yaml')
  yaml = '~' + src_dir + '/k8s-conf.generated.yaml'

  # print(yaml)
  return k8s_service(yaml, image)

