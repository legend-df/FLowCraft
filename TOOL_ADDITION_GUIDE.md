# 工具添加指南 / Tool Addition Guide

本文档说明如何在FlowCraft中添加新的工具。  
This document explains how to add new tools in FlowCraft.

## 当前工具 / Current Tools

FlowCraft目前包含以下工具：
- **Select** - 选择和拖拽图元 / Select and drag graphics items
- **DeleteLine** - 删除连接线 / Delete connection lines  
- **DeleteNode** - 删除节点 / Delete nodes

## 添加步骤 / Addition Steps

### 1. 定义枚举 / Define Enum

在 `Globe/Enum.h` 中的 `ToolType` 枚举中添加新工具：

```cpp
enum ToolType
{
    Select,
    DeleteLine,
    DeleteNode,
    NewTool  // 添加新工具 / Add new tool
};
```

### 2. 准备图标 / Prepare Icon

将工具图标（PNG格式）添加到资源文件，路径通常为 `:/NodeData/icon/`

### 3. 创建UI按钮 / Create UI Button

在 `UI/Editor/MainWindow.cpp` 构造函数中添加：

```cpp
// 创建工具按钮 / Create tool button
QToolButton* newTool = new QToolButton();
newTool->setIcon(QIcon(":/NodeData/icon/newTool.png"));
ui->toolBar->addWidget(newTool);

// 连接事件 / Connect event
connect(newTool, &QToolButton::clicked, [=]() {
    QPixmap cursorPixmap(":/NodeData/icon/newTool.png");
    cursorPixmap = cursorPixmap.scaled(32, 32);
    QCursor customCursor(cursorPixmap, 0, 0);
    QApplication::setOverrideCursor(customCursor);
    view->Tool = NewTool;
});
```

### 4. 实现功能 / Implement Functionality

在 `Graphics/GraphicsView/Graphicsview.cpp` 中实现工具逻辑：

```cpp
void GraphicsView::mousePressEvent(QMouseEvent* event)
{
    // ... 现有代码 / existing code ...
    
    if (Tool == NewTool)
    {
        // 实现新工具逻辑 / Implement new tool logic
        handleNewToolAction(event);
    }
}
```

## 最佳实践 / Best Practices

1. **运行时检查** / Runtime Check: 检查 `RuningConfig::IsRuning` 状态
2. **音频反馈** / Audio Feedback: 使用 `AudioPlayer::Play()` 提供反馈
3. **用户提示** / User Tips: 使用 `GraphicsTip::showTips()` 显示提示
4. **代码风格** / Code Style: 遵循现有的命名约定和风格

## 测试清单 / Testing Checklist

- [ ] 编译成功 / Compilation successful
- [ ] 工具按钮显示正确 / Tool button displays correctly
- [ ] 基本功能工作 / Basic functionality works
- [ ] 工具切换正常 / Tool switching works
- [ ] 运行时行为正确 / Runtime behavior correct
- [ ] 光标和反馈正常 / Cursor and feedback working

## 注意事项 / Notes

- 修改枚举时注意兼容性 / Be careful about compatibility when modifying enums
- 确保资源文件正确包含 / Ensure resource files are properly included
- 测试与现有功能的交互 / Test interaction with existing features
- 考虑撤销/重做支持 / Consider undo/redo support

---

*FlowCraft 开发文档 / FlowCraft Development Documentation*