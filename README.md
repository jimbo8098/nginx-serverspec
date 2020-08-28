An nginx image with added net-tools package in order to ensure you can run network diagnostics required for serverspec to check ports are open.

The image is used with ruby gems:

```
serverspec
docker-api
specinfra-backend-docker_compose
```

Alongside this, it is important that you have your rspec set with:

```
set :os, family: :debian
```

An example configuration which should work with nothing more is listed below:

```
require 'serverspec'
require 'specinfra/backend/docker_compose'

set :os, family: :debian
set :docker_compose_file, './docker-compose.yml'
set :docker_compose_container, :nginx
set :docker_wait, 3
set :backend, :docker_compose

#File.write('./nginx.conf','')
describe 'docker-compose.yml run' do
  describe port(80) do
      it { should be_listening }
  end
end
```

Assuming a minimal docker-compose.yml file exists with contents:

```
nginx:
  image: nginx-serverspec
```
