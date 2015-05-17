# Install spark with native libraries and lzo file support.

# Navigate to user folder 
cd /home/user

# Clone git repo with some peliminary configurations
git clone git@github.com:kmetsalu/spark-with-lzo.git spark-with-lzo
cd /home/user/spark-with-lzo

# Export BASE_DIR variable for easier use.
export $BASE_DIR=`pwd`

# Download Spark 1.2.0
wget http://www.apache.org/dyn/closer.cgi/spark/spark-1.2.0/spark-1.2.0-bin-hadoop2.4.tgz
tar –xzvf spark-1.2.0-bin-hadoop2.4.tgz
mv spark-1.2.0-bin-hadoop2.4 spark

# Download hadoop-lzo
git clone https://github.com/twitter/hadoop-lzo.git hadoop-lzo
cd $BASE_DIR/hadoop-lzo
git checkout release-0.4.19

# ===
# Build native libraries according to hadoop-lzo instructions
# After build there should be target directory in hadoop-lzo folder
# ===

# Make new directory to store current compilaton
mkdir $BASE_DIR/hadoop-lzo/current

# Copy required native libraries to created directory for Linux 
# folder name within native directory differs, but everything else
# is the same
cp –R $BASE_DIR/hadoop-lzo/target/native/Mac_OS_X-x86_64-64/lib/ $BASE_DIR/hadoop-lzo/current/native

# Copy created jar
cp $BASE_DIR/hadoop-lzo/target/hadoop-lzo-0.4.19.jar $BASE_DIR/hadoop-lzo/current

# Copy configuration files to spark
cd $BASE_DIR
cp $BASE_DIR/spark-conf/conf/* $BASE_DIR/spark/conf/
mv $BASE_DIR/spark/sbin/spark-config.sh{,.backup}
cp $BASE_DIR/spark-cond/sbin/* $BASE_DIR/spark/sbin/
# Native libraries setup should be now working