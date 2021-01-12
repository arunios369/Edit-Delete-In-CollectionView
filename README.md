# Edit-Delete-In-CollectionView
Edit &amp; Delete In CollectionView By Using Protocol


import UIKit

class DemoViewController: UIViewController ,UICollectionViewDelegate,UICollectionViewDataSource, DataCollectionProtocal {
    
    var array:[String] = ["Prabhas", "Anushka", "Rana","Tamannaah", "Adivi sesh","Nassar","Ramya","Sathyaraj","Subbaraju","Sudeep"]
    var imgarray:[UIImage] = [#imageLiteral(resourceName: "prabhas"),#imageLiteral(resourceName: "anushka"),#imageLiteral(resourceName: "rana"),#imageLiteral(resourceName: "tamannaah"),#imageLiteral(resourceName: "adivi-sesh"),#imageLiteral(resourceName: "nassar"),#imageLiteral(resourceName: "ramya-krishnan"),#imageLiteral(resourceName: "sathyaraj"),#imageLiteral(resourceName: "subbaraju"),#imageLiteral(resourceName: "sudeep")]
    
    @IBOutlet weak var collectionView: UICollectionView!
    var inputTextField = UITextField()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        
    }
    func collectionView(_ collectionView: UICollectionView, numberOfItemsInSection section: Int) -> Int {
        return array.count
    }
    
    func collectionView(_ collectionView: UICollectionView, cellForItemAt indexPath: IndexPath) -> UICollectionViewCell {
        let cell = collectionView.dequeueReusableCell(withReuseIdentifier: "cell", for: indexPath) as! DemoCollectionViewCell
        let images = imgarray[indexPath.row]
        cell.img.image = images
        cell.lbl.text = array[indexPath.row]
        cell.index = indexPath
        cell.data = self
        
        return cell
        
    }
    
    
    func collectionView(_ collectionView: UICollectionView, didSelectItemAt indexPath: IndexPath) {

        let vc = storyboard?.instantiateViewController(withIdentifier: "DetailsColletionViewController") as! DetailsColletionViewController
        vc.getLabel = array[indexPath.row]
        vc.getImage = imgarray[indexPath.row]
        self.navigationController?.pushViewController(vc, animated: true)

    }
    
    //edit button
    func passData(indx: Int) {
      
        let alert = UIAlertController(title: "EDIT", message: "Enter Here", preferredStyle: .alert)
        let yes = UIAlertAction(title: "YES", style: .destructive) { (UIAlertAction) in
            
            let textData = self.inputTextField.text
            self.array[indx] = textData!
            self.collectionView.reloadData()
        }
        let no = UIAlertAction(title: "NO", style: .cancel, handler: nil)
        alert.addAction(yes)
        alert.addAction(no)
        alert.addTextField(configurationHandler: inputTextField)
        
        self.present(alert, animated: true, completion: nil)
    }

    // delete button
    func deleteData(indx: Int)
    {
        let alert = UIAlertController(title: "Delete", message: "Confirm to delete", preferredStyle: .actionSheet)
        let yes = UIAlertAction(title: "yes", style: .destructive) { (UIAlertAction) in
            
            self.imgarray.remove(at: indx)
            self.array.remove(at: indx)
            self.collectionView.reloadData()
        }
        let no = UIAlertAction(title: "No", style: .cancel, handler: nil)
        alert.addAction(no)
        alert.addAction(yes)
        self.present(alert, animated: true, completion: nil)
       
    }
    func inputTextField(textField: UITextField) {
        inputTextField = textField
        inputTextField.placeholder = "enter"
        inputTextField.clearButtonMode = .whileEditing
    }


    
   
}
// tableView Cell
import UIKit

protocol DataCollectionProtocal {
    func passData(indx: Int)
    func deleteData(indx: Int)
}
class DemoCollectionViewCell: UICollectionViewCell {
    
    @IBOutlet var deleteOutlet: UIButton!
    @IBOutlet var editOutlet: UIButton!
    @IBOutlet weak var lbl: UILabel!
    @IBOutlet weak var img: UIImageView!
    
    var data: DataCollectionProtocal?
    var index: IndexPath?
    
    @IBAction func editCell(_ sender: UIButton) {
        data?.passData(indx: index!.row)
    }
    
    @IBAction func deleteCell(_ sender: UIButton) {
    
        data?.deleteData(indx: index!.row)
        
    }
}

//Detail view controller


import UIKit

class DetailsColletionViewController: UIViewController {

    @IBOutlet weak var label: UILabel!
    @IBOutlet weak var imageView: UIImageView!
   
    
    var getLabel = String()
    var getImage = UIImage()
    override func viewDidLoad() {
        super.viewDidLoad()
        
        
        label.text = getLabel
        imageView.image = getImage
    }
    

   
}


