- 因断电发生坏块怎么办
-- 检测路径下是否有坏块
bin/hdfs fsck /
-- 删除路径下的坏块
bin/hdfs fsck / -delete
-- 修复坏块
hdfs debug recoverLease -path /  -retries 5