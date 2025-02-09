# Doc_SwitchTreeView
You can show switch document
![image](https://github.com/user-attachments/assets/f108dbb5-1664-42a7-a659-0bf4704d1c6d)
\\\Cs
 private TreeView treeView11;
 private Button showCheckedNodesButton;
 private TreeViewCancelEventHandler checkForCheckedChildren;

 public Form1()
 {
     treeView11 = new TreeView();
     showCheckedNodesButton = new Button();
     checkForCheckedChildren =
         new TreeViewCancelEventHandler(CheckForCheckedChildrenHandler);

     this.SuspendLayout();

     // Initialize treeView1.
     treeView11.Location = new Point(0, 25);
     treeView11.Size = new Size(292, 248);
     treeView11.Anchor = AnchorStyles.Top | AnchorStyles.Left |
         AnchorStyles.Bottom | AnchorStyles.Right;
     treeView11.CheckBoxes = true;

     // Add nodes to treeView1.
     TreeNode node;
     for (int x = 0; x < 3; ++x)
     {
         // Add a root node.
         node = treeView11.Nodes.Add(String.Format("Node{0}", x * 4));
         for (int y = 1; y < 4; ++y)
         {
             // Add a node as a child of the previously added node.
             node = node.Nodes.Add(String.Format("Node{0}", x * 4 + y));
         }
     }

     // Set the checked state of one of the nodes to
     // demonstrate the showCheckedNodesButton button behavior.
     treeView11.Nodes[1].Nodes[0].Nodes[0].Checked = true;

     // Initialize showCheckedNodesButton.
     showCheckedNodesButton.Size = new Size(144, 24);
     showCheckedNodesButton.Text = "Show Checked Nodes";
     showCheckedNodesButton.Click +=
         new EventHandler(showCheckedNodesButton_Click);

     // Initialize the form.
     this.ClientSize = new Size(292, 273);
     this.Controls.AddRange(new Control[]
         { showCheckedNodesButton, treeView11 });

     this.ResumeLayout(false);
 }
 private void showCheckedNodesButton_Click(object sender, EventArgs e)
 {
     // Disable redrawing of treeView1 to prevent flickering 
     // while changes are made.
     treeView11.BeginUpdate();

     // Collapse all nodes of treeView1.
     treeView11.ExpandAll();

     // Add the checkForCheckedChildren event handler to the BeforeExpand event.
     treeView11.BeforeCollapse += checkForCheckedChildren;

     // Expand all nodes of treeView1. Nodes without checked children are 
     // prevented from expanding by the checkForCheckedChildren event handler.
     treeView11.CollapseAll();

     // Remove the checkForCheckedChildren event handler from the BeforeExpand 
     // event so manual node expansion will work correctly.
     treeView11.BeforeCollapse -= checkForCheckedChildren;

     // Enable redrawing of treeView1.
     treeView11.EndUpdate();
 }

 // Prevent collapse of a node that has checked child nodes.
 private void CheckForCheckedChildrenHandler(object sender,
     TreeViewCancelEventArgs e)
 {
     if (HasCheckedChildNodes(e.Node)) e.Cancel = true;
 }

 // Returns a value indicating whether the specified 
 // TreeNode has checked child nodes.
 private bool HasCheckedChildNodes(TreeNode node)
 {
     if (node.Nodes.Count == 0) return false;
     foreach (TreeNode childNode in node.Nodes)
     {
         if (childNode.Checked) return true;
         // Recursively check the children of the current child node.
         if (HasCheckedChildNodes(childNode)) return true;
     }
     return false;
 }

\\\

