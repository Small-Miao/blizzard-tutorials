# 导入器

导入器允许你把标准《星际争霸》资源库之外的自定义资源整合进项目中。

# 导航导入器

通过 `Module ▶︎ Importer` 打开导入器。你会看到如下界面。

[![The Importer](./resources/014_The_Importer01.png)](./resources/014_The_Importer01.png)
*导入器*

左侧是名为 “Document Files” 的子视图，这里会按各自目录列出所有已导入文件。右侧则是名为 “File Info” 的子视图，用于显示当前文件的详细信息。一个已载入项目中的导入器大致如下图所示。

[![Importer in an Active Project](./resources/014_The_Importer02.png)](./resources/014_The_Importer02.png)
*活动项目中的导入器*

如你所见，即使文件已经导入到项目中，它们仍会在导入器里保留原本的文件结构。虽然名字看上去像是一个专门用来添加新文件的工具，但导入器实际上也像是一种“导入模块”，你可以通过它查看所有来自引擎外部的资源，并了解它们当前的文件结构。

## 导入文件

在 “Document File” 区域右键并选择 `Import Files` 来导入文件。这会打开如下所示的“导入文件”窗口。

[![Importing Files](./resources/014_The_Importer03.png)](./resources/014_The_Importer03.png)
*导入文件*

这个窗口会自动填入所选目录中的全部文件。每个被勾选的文件都会在你点击 “OK” 后导入导入器。你可以通过勾选或取消勾选文件夹层级顶部的项，来一次性启用或停用某个文件夹中的所有文件。

勾选最顶层文件夹，本例中是 `Desktop`，就会让该目录下的所有文件都准备好导入。完成选择后，你可以通过 “Import Path” 字段设置这些文件在导入器中的路径。上图使用的是来自暴雪自定义地图 StarJeweled 的资源，它会被导入到 `Assets/纹理` 路径下。这会把纹理 `StarJeweled_Gem_Black.dds` 放进《星际争霸》资源结构中一个预先存在的位置。

## 已导入文件目录

回到导入器后，你会看到文件名显示为绿色。这表示文件已导入，但尚未保存。导入器中还存在其他几种颜色状态，见下表。

| 颜色 | 状态 |
| ----- | ---------------------------------------------- |
| 绿色 | 文件已导入，但尚未保存。 |
| 红色 | 文件已移除，但尚未保存。 |
| 蓝色 | 文件已移动或重命名，但尚未保存。 |
| 黑色 | 文件已保存。 |

此时你应当保存一次，以便把文件正式固定在项目文件结构中的当前位置，如下图所示。

[![Imported File Structure](./resources/014_The_Importer04.png)](./resources/014_The_Importer04.png)
*导入后的文件结构*

接下来，确认文件是否位于正确目录。请记住，这个文件本应被设置到 `Assets/纹理` 文件夹下，但它似乎仍然保留了原始位置中的 `StarJeweled Assets` 文件夹。你可以使用导入器的文件移动功能来修正这一点。

## 移动文件

右键点击某个文件并选择 `Move Files` 即可移动它。执行后会出现 “Move Files” 窗口。该窗口允许你将资源移动到 “Existing Path” 或 “New Path”。请选择 “New Path” 并输入 `Assets/纹理`，把文件移动到它原本应该被放入的目录中。

![Altering a File Path](./resources/014_The_Importer05.png)
*修改文件路径*

保存文件后再次查看导入器，应当能看到如下结果。

![Imported File with Corrected Directory](./resources/014_The_Importer06.png)
*目录已修正的导入文件*

现在这个文件看起来已经位于正确位置了。为了最终确认，你可以通过 `Window ▶︎ Console` 打开 Archive Browser。然后在控制台中输入 `browse`。使用浏览器的搜索功能，输入资源名称来确认它在文件结构中的位置。结果如下图所示。

![](./resources/014_The_Importer07.png)
*Archive Browser 确认文件结构正确*
