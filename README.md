    //Types of Mark in Xcode

    // MARK:data
    // TODO:data
    // FIXME:data
    // ???:data
    // !!!:data

#### UITableView with CustomCell

    override func viewDidLoad() {
    super.viewDidLoad()
     //--------------------------   RegisterCell without XIB   -------------------------------------//
     // Register Cell Class
     tableView.register(CustomTableViewCell.self, forCellReuseIdentifier:
     CustomTableViewCell.identifier)
      //-----------------------------  RegisterCell with XIB   --------------------------------------//
      
      // Register Nib
      tableView.register(UINib(nibName: CustomTableViewCell.identifier, bundle: nil),
      forCellReuseIdentifier: CustomTableViewCell.identifier)
       }
    
    //--------------------------  TableView DataSource and Delegate Methods --------------------------//
    extension GoalsVc: UITableViewDelegate, UITableViewDataSource {
    func numberOfSections(in tableView: UITableView) -> Int {
        return 1
    }
    
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return arrGoal.count
    }
    
    
    func tableView(_ tableView: UITableView, viewForHeaderInSection section: Int) -> UIView? {
      let view = UIView(frame: CGRect(x: 0, y: 0, width: tableView.frame.size.width, height: 22))
      view.backgroundColor = UIColor.groupTableViewBackgroundColor()
      let label = UILabel()
      label.font = UIFont.systemFontOfSize(12)
      label.textColor = UIColor.darkGrayColor()
      switch section {
      case 1:
      label.text = "Title"
      label.frame = labelFrame
      let more = UIButton(frame: btnFrame)
      more.setTitle("See more", forState:.Normal)
      view.addSubview(more)
      default:
      label.frame = CGRect.zero
      }
      view.addSubview(label)
      return view;
    }
---    

     func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        guard  let cell = tableView.dequeueReusableCell(withIdentifier: "GoalCell", for: indexPath) as? GoalCell else {return UITableViewCell()}
        let goal = arrGoal[indexPath.row]
         cell.configureCell(goal: goal)
       // cell.configureCell(description: "Salad", type: "ShortTearm", goalProgressAmount: 100)
        
        return cell
    }
---   

    //With this we can edit UITableview ex. Swipe to Delete
    func tableView(_ tableView: UITableView, canEditRowAt indexPath: IndexPath) -> Bool {
        return true
    }
    
    //Select tableview Editing Style (insert and Delete)-> if custom icon than set None
    func tableView(_ tableView: UITableView, editingStyleForRowAt indexPath: IndexPath) -> UITableViewCellEditingStyle {
        return UITableViewCellEditingStyle.none
    }
---    

    //Delete Action
    1) Create delete Action 2) Remove data with Indexpath 3) fetch data from coredata 4) delete tableview        row 4) set  delete button background color 5) return deleteAction in arry wether it is single
    func tableView(_ tableView: UITableView, editActionsForRowAt indexPath: IndexPath) -> [UITableViewRowAction]? {
        //Destructive Because we want to delete(destroy) the data from tableview
        let deleteAction = UITableViewRowAction(style: .destructive, title: "DELETE") { (rowAction, indexpath) in
            self.removeGoal(atIndexPath: indexpath)
            self.fetchCoreDataObjects()
            tableView.deleteRows(at: [indexpath], with: .automatic)
        }
        
        let addAction = UITableViewRowAction(style: .normal, title: "ADD 1") { (rowAction, indexpath) in
            self.setProgress(atIndexpath: indexPath)
            tableView.reloadRows(at: [indexPath], with: .automatic)
        }
        deleteAction.backgroundColor = #colorLiteral(red: 1, green: 0.1491314173, blue: 0, alpha: 1)
        addAction.backgroundColor = #colorLiteral(red: 0.9176470588, green: 0.662745098, blue: 0.2666666667, alpha: 1)
        return [deleteAction,addAction]
    }
    }
    
#### UICollectionView With CustomCell

     // Create CollectionView Programatically
      func createCollectionView() {
         let layout: UICollectionViewFlowLayout = UICollectionViewFlowLayout()
         let collectionView = UICollectionView(frame: CGRect(x: 0, y: 0, width: view.frame.width,
         height: view.frame.height), collectionViewLayout: layout)
         collectionView.dataSource = self
         collectionView.delegate = self
         view.addSubview(collectionView)
     }
---

    extension ProductsVC: UICollectionViewDelegate, UICollectionViewDataSource{

    // MARK: - UICollectionViewDataSource
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section:
    Int) -> Int {
     guard collectionView == collectionCategory else {
     return arrOfProducts.count
     }
    return arrOfCategory.count
    }

    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath)
    -> UICollectionViewCell {
      guard collectionView == collectionProduct else {
      let cell = collectionView.dequeueReusableCell(withReuseIdentifier:
      "ProductCategoryCell", for: indexPath) as! ProductCategoryCell
      cell.viewBackground.layer.borderWidth = 0.5
      //Do some thing as per use
      return cell
    }
     let cell = collectionView.dequeueReusableCell(withReuseIdentifier: cellIdentifier, for:
    indexPath) as! ProductCell
    cell.contentView.layer.borderWidth = 0.5
    cell.contentView.layer.borderColor = UIColor.black.cgColor
    let json = arrOfProducts[indexPath.row]
    //Do something as per use
    return cell
    }

    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath:
    IndexPath) {
      guard collectionView == collectionCategory else {
      let json = arrOfProducts[indexPath.row]
      // Do something for collectionProduct here
      return
    }
    let json = arrOfCategory[indexPath.row] as [String: AnyObject]
    let id = json["cId"] as? String ?? ""
    // Do something
     }
    }
---

    extension ProductsVC: UICollectionViewDelegateFlowLayout{

    // MARK: - UICollectionViewDelegateFlowLayout
    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout:
    UICollectionViewLayout, sizeForItemAt indexPath: IndexPath) -> CGSize {
       let collectionWidth = collectionView.bounds.width
       guard collectionView == collectionProduct else {
       var itemWidth = collectionWidth / 4 - 1;
       if(UI_USER_INTERFACE_IDIOM() == .pad) {
       itemWidth = collectionWidth / 4 - 1;
       }
       return CGSize(width: itemWidth, height: 50)
      }
      var itemWidth = collectionWidth / 2 - 1;
      if(UI_USER_INTERFACE_IDIOM() == .pad) {
      itemWidth = collectionWidth / 4 - 1;
      }
      return CGSize(width: itemWidth, height: 250);
    }

    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout:
    UICollectionViewLayout, minimumInteritemSpacingForSectionAt section: Int) -> CGFloat {
      return 1
    }

    func collectionView(_ collectionView: UICollectionView, layout collectionViewLayout:
    UICollectionViewLayout, minimumLineSpacingForSectionAt section: Int) -> CGFloat {
       return 1
     }
    }

# UICOLLECTIONVIEW
=============================
=> ACCESS UICOLLECTIONVIEWCELL ON UIBUTTON CLICK :-
   ``` NSIndexPath *indexPath;
    indexPath = [self.aCollectionview indexPathForItemAtPoint:[self.aCollectionview convertPoint:sender.center fromView:sender.superview]];
    CLCellPastMatch *cell = (CLCellPastMatch *)[self.aCollectionview cellForItemAtIndexPath:indexPath];```
    //CLCellPastMatch is UICollectionview cell and Sender is your UIButton on UICollectionview Cell
