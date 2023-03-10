---
sidebar_position: 0
---

# Installation and Setup

We'll first need to get you set up with some tools and configurations to ensure a smooth development experience. For a more comprehensive overview or detailed explanation on any of the following, please reference the corresponding official documentation.

### Clone the repository

To get started, clone the following repositories

```
$ git clone https://github.com/acmutd/portal-next.git
$ git clone https://github.com/acmutd/portal-serverless.git
$ git clone https://github.com/acmutd/portal-docs.git
```

### npm

Ensure that you're using npm version of at least 8.6.0. There's a bug on previous versions that prevents npm from resolving local package dependencies within the npm workspace.

To update npm to the latest version, run `npm install -g npm@latest` or if you have [Node Version Manager](https://github.com/nvm-sh/nvm) installed `nvm install-latest-npm`

### node

All repositories in the ACM Member Portal Project requires node version at least 16.14.

### Global packages

`portal-next` requires the following global npm packages:

- Prisma
- Doppler

These packages can be installed as follow:

```
$ npm install -g prisma
$ npm install -g doppler
```

### Doppler

Doppler is a universal secrets platform, which streamlines secret management with a dashboard, CLI, and integrations for syncing secrets across development environments, cloud providers, hosting platforms, CI/CD tools, Terraform, and more. The Doppler CLI will provide access to secrets, and will inject them as environment variables into the running process from your command or script.

The next steps differ slightly for officers and for contributors.

#### Officers

- Install the Doppler CLI. This step varies depending on your operating system, so please refer to the offical [Doppler Install CLI](https://docs.doppler.com/docs/install-cli#installation) documentation for your specific installation steps
- [Login](https://docs.doppler.com/docs/install-cli#authentication) to the CLI. This will open up a browser window and ask you to authenticate; log in with your ACM email
  ```
  doppler login
  ```
- Configure Doppler by running the following in the root of the repository
  ```
  doppler setup
  ```
- Select project `portal`
- Select config `next-dev_local`

#### Contributors

- Reach out to `development@acmutd.co` to receive a Doppler service token
- Install the Doppler CLI. This step varies depending on your operating system, so please refer to the offical [Doppler Install CLI](https://docs.doppler.com/docs/install-cli#installation) documentation for your specific installation steps
- Configure Doppler by running the following in the root of the repository

  ```
  # Prevent configure command being leaked in bash history
  export HISTIGNORE='doppler*'

  # Scope to location of application directory
  echo 'dp.st.prd.xxxx' | doppler configure set token --scope /usr/src/app

  # Supply secrets to your application
  cd /usr/src/app
  doppler run -- your-command-here
  ```

Doppler will only inject the secrets as environment variables for commands or scripts which are prepended with `doppler run -- `. This is already prepended onto scripts in package.json, so you don't need to worry about prepending this yourself.

### Building the Project

To install all dependencies, run the following command:

```
$ npm install
```

To start `portal-next`, run the following commands:

```
$ npm run prisma:generate
$ npm run dev
```

To build `portal-next`, run the following commands:

```
$ npm run prisma:generate
$ npm run build
```

:::note
`npm run prisma:generate` is only required to be run after installing new dependencies or updating the Prisma schema.
:::
