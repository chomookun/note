# install

## edit profile
```bash
vim ~/.bash_profile
...
alias python='python3'
alias pip='pip3'
export LD_LIBRARY_PATH=/usr/local/lib:$LD_LIBRARY_PATH
...

$ source ~/.bash_profile
```

# install airflow
```bash
export AIRFLOW_HOME=~/airflow
AIRFLOW_VERSION=2.2.2
PYTHON_VERSION="$(python --version | cut -d " " -f 2 | cut -d "." -f 1-2)"
echo ${PYTHON_VERSION}

CONSTRAINT_URL="https://raw.githubusercontent.com/apache/airflow/constraints-${AIRFLOW_VERSION}/constraints-${PYTHON_VERSION}.txt"
pip install "apache-airflow==${AIRFLOW_VERSION}" --constraint "${CONSTRAINT_URL}" 
```

# in case) sqlite
```bash
sudo yum -y install wget tar gzip gcc make expect
wget https://www.sqlite.org/src/tarball/sqlite.tar.gz
tar xzf sqlite.tar.gz
cd sqlite/
export CFLAGS="-DSQLITE_ENABLE_FTS3 \
    -DSQLITE_ENABLE_FTS3_PARENTHESIS \
    -DSQLITE_ENABLE_FTS4 \
    -DSQLITE_ENABLE_FTS5 \
    -DSQLITE_ENABLE_JSON1 \
    -DSQLITE_ENABLE_LOAD_EXTENSION \
    -DSQLITE_ENABLE_RTREE \
    -DSQLITE_ENABLE_STAT4 \
    -DSQLITE_ENABLE_UPDATE_DELETE_LIMIT \
    -DSQLITE_SOUNDEX \
    -DSQLITE_TEMP_STORE=3 \
    -DSQLITE_USE_URI \
    -O2 \
    -fPIC"
export PREFIX="/usr/local"
LIBS="-lm" ./configure --disable-tcl --enable-shared --enable-tempstore=always --prefix="$PREFIX"
make
make install
```

# in case) Mysql
```bash
# install airflow mysql plugin
sudo yum install python3-devel.x86_64
pip3 install 'apache-airflow[mysql]'

# edit config
vim airflow.cfg
...
# metadata connect
sql_alchemy_conn = mysql://[user]:[password]@localhost:3306/airflow

# reload plugins
reload_on_plugin_change = True
...
```


# setup
```bash
# initialize db
airflow db init

# create user
airflow users create \
    --username admin \
    --firstname admin \
    --lastname admin \
    --role Admin \
    --email admin@undefined.org
```

# startup
```bash
# start
airflow standalone
```