

```
for n in www bbs chat;
do
mkdir -p ../html/$n;
echo "http://${n}.abcd.com" > ../html/$n/index.html;
cat ../html/$n/index.html;
done
```
