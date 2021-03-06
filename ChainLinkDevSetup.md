# Set up a ChainLink Development Environment

## Install required packages

Ensure that your system is up to date and fully patched

```script
sudo apt update -y && sudo apt upgrade -y
```
Install dependencies

```script
sudo apt -y install build-essential git libssl1.0-dev libxml2-dev libxslt1-dev libreadline-dev zlib1g-dev postgresql-9.6 libpq-dev nodejs
```

## Set up Ruby

Clone the repository

```script
git clone git://github.com/sstephenson/rbenv.git $HOME/.rbenv
```

Set up local environment variables

```script
echo 'export PATH="$HOME/.rbenv/bin:$PATH"' >> $HOME/.bashrc
echo 'eval "$(rbenv init -)"' >> $HOME/.bashrc
exec $SHELL -l
```

Get and set up the build

```script
git clone git://github.com/sstephenson/ruby-build.git $HOME/.rbenv/plugins/ruby-build
rbenv install 2.3.5
rbenv rehash
rbenv global 2.3.5
ruby -v
```

## Set up PostgreSQL

Substitute "thomas" for your own username

(If you accidentally hit Enter on the SQL commands without the semicolon, type \g to commit the command)

```script
su -
su postgres
psql
CREATE ROLE thomas LOGIN INHERIT;
ALTER ROLE thomas CREATEDB;
\q
exit
exit
```

## Finally get and install ChainLink

```script
git clone https://github.com/smartcontractkit/chainlink.git && cd chainlink
gem install bundler && bundle
rake db:create db:migrate
rake oracle:initialize
```

Start the server

```script
foreman start
```

## Testing
 
To run the full test suite, including integration tests, you need an instance of DevNet running on your machine. This requires first installing [Parity](https://github.com/paritytech/parity). Once Parity is installed, run the following commands:

```script
git clone https://github.com/smartcontractkit/devnet.git
cd devnet
./start
```

Then to run the full test suite run:

```script
rake
```

Or test a specific test:

```script
rspec spec/models/assignment_spec.rb:57
```
