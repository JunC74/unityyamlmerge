# unityyamlmerge
unity smart merge 在自定义的合并驱动中的设置

在.gitconfig中配置自定义的合并驱动

[merge unityyamlmerge]
  name = A custom merge driver used to resolve conflicts in certain files
	driver = {unity smart merge 程序的路径} merge -p --force %O %B %A %A
 
在.gitattributes中配置yaml格式的文件合并驱动设置
*.mat merge=unityyamlmerge eol=lf
*.anim merge=unityyamlmerge eol=lf
*.unity merge=unityyamlmerge eol=lf
*.prefab merge=unityyamlmerge eol=lf
*.physicsMaterial2D merge=unityyamlmerge eol=lf
*.physicsMaterial merge=unityyamlmerge eol=lf
*.asset merge=unityyamlmerge eol=lf
*.meta merge=unityyamlmerge eol=lf
*.controller merge=unityyamlmerge eol=lf

然后git pull 或 git merge 就能自动使用untiy的smart merge来合并场景的差异，在一定程度上可以解决场景的冲突问题
