# Unity YAML Merge

unity smart merge 在自定义的合并驱动中的设置

## 在.gitconfig中配置自定义的合并驱动

``` 
[merge unityyamlmerge]
	name = A custom merge driver used to resolve conflicts in certain files
	driver = {unity smart merge 程序的路径} merge -p --force %O %A %B %A
``` 
note:

* %O %A %B 的意思请看 https://git-scm.com/docs/gitattributes 中关于Defining a custom merge driver一节的内容
* merge -p --force 是UnityYAMLMerge的参数，可以直接运行UnityYAMLMerge来查看
 
## 在.gitattributes中配置yaml格式的文件合并驱动设置

``` 
*.mat merge=unityyamlmerge eol=lf
*.anim merge=unityyamlmerge eol=lf
*.unity merge=unityyamlmerge eol=lf
*.prefab merge=unityyamlmerge eol=lf
*.physicsMaterial2D merge=unityyamlmerge eol=lf
*.physicsMaterial merge=unityyamlmerge eol=lf
*.asset merge=unityyamlmerge eol=lf
*.meta merge=unityyamlmerge eol=lf
*.controller merge=unityyamlmerge eol=lf

``` 

然后git pull 或 git merge 就能自动使用untiy的smart merge来合并场景的差异，在一定程度上可以解决场景的冲突问题

## 关于合并中发生的冲突处理

UnityYAMLMerge如果检测到修改的冲突，会用一个指定的比较工具来交由用户自己处理冲突部分的取舍。

默认的比较工具配置在UnityYAMLMerge工具的目录下的mergespecfile.txt文件中,如果用户需要自己调整哪个工具的优先级或者添加定制的工具都可以自己调整，然后通过UnityYAMLMerge的“--fallback”参数来指定配置文件的路径。

如：在mac下采用Beyond Compare来手动合并的冲突的配置如下

```
#
# UnityYAMLMerge fallback file
#

# Modify the next two lines if scene or prefab files should fallback
# on other that the default fallbacks listed below.
#
# %l is replaced with the path of you local version
# %r is replaced with the path of the incoming remote version
# %b is replaced with the common base version
# %d is replaced with a path where the result should be written to
# On Windows %programs% is replaced with "C:\Program Files" and "C:\Program Files (x86)" there by resulting in two entries to try out
# On OSX %programs% is replaced with "/Applications" and "$HOME/Applications" thereby resulting in two entries to try out

unity use "%programs%\YouFallbackMergeToolForScenesHere.exe" "%l" "%r" "%b" "%d"
prefab use "%programs%\YouFallbackMergeToolForPrefabsHere.exe" "%l" "%r" "%b" "%d"

#
# Default fallbacks for unknown files. First tool found is used.
#

# Beyond Compare
* use "%programs%/Beyond Compare.app/Contents/MacOS/bcomp" "%b" "%l" "%r" "%d" -lefttitle="Base Version" -righttitle="Remote Version" -centertitle="Local Version" -outputtitle="Out Put"

```
