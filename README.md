不是男同
我想找人帮忙开发前列腺
我在河南
可以加我qq1069059564
需要到医院体检或者试纸
性病勿扰
name: Build BBS

on:
  push:
    branches: [master]

env:
  FLARUM: ghcr.io/viva-la-vita/flarum
  NGINX: ghcr.io/viva-la-vita/nginx

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: Login to ghcr.io
        uses: docker/login-action@v2
        with:
          registry: ghcr.io
          username: viva-la-vita
          password: ${{ secrets.GITHUB_TOKEN }}
      - name: Set up Docker Buildx
        uses: docker/setup-buildx-action@v2
      - name: Build and push Flarum
        uses: docker/build-push-action@v2
        with:
          context: ./flarum
          push: true
          tags: ${{ env.FLARUM }}
          target: production
          cache-from: type=registry,ref=${{ env.FLARUM }}:buildcache
          cache-to: type=registry,ref=${{ env.FLARUM }}:buildcache,mode=max
      - name: Build and push Nginx
        uses: docker/build-push-action@v2
        with:
          context: ./nginx
          push: true
          tags: ${{ env.NGINX }}
          target: production
          cache-from: type=registry,ref=${{ env.NGINX }}:buildcache
          cache-to: type=registry,ref=${{ env.NGINX }}:buildcache,mode=max
