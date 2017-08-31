//Types of Mark in Xcode


```// MARK:data
// TODO:data
// FIXME:data
// ???:data
// !!!:data```

# UICOLLECTIONVIEW
=============================
=> ACCESS UICOLLECTIONVIEWCELL ON UIBUTTON CLICK :-
   ``` NSIndexPath *indexPath;
    indexPath = [self.aCollectionview indexPathForItemAtPoint:[self.aCollectionview convertPoint:sender.center fromView:sender.superview]];
    CLCellPastMatch *cell = (CLCellPastMatch *)[self.aCollectionview cellForItemAtIndexPath:indexPath];```
    //CLCellPastMatch is UICollectionview cell and Sender is your UIButton on UICollectionview Cell
