/////////////---------_Code_---------///////////////////
1 Enable the use of moving items in the UICollectionView by calling canMoveItemAt delegate method. Passing true will enable this feature.

func collectionView(_ collectionView: UICollectionView, canMoveItemAt indexPath: IndexPath) -|gt| Bool {
        return true
    }

2 Next, we will implement the moveItemAt delegate method where you will intercept the starting index and the ending index of the both items that are switching places.
import UIKit

func collectionView(_ collectionView: UICollectionView, moveItemAt sourceIndexPath: IndexPath, to destinationIndexPath: IndexPath) {
        let item = str_arr.remove(at: sourceIndexPath.item)
        str_arr.insert(item, at: destinationIndexPath.item)
        print(str_arr)
    }

3 UILongPressGestureRecognizer, For a better control of the gestures in the UICollectionView we will implement a UILongPressGestureRecognizer.

fileprivate var longPressGesture: UILongPressGestureRecognizer!
    @IBOutlet weak var collectionview: UICollectionView!

override func viewDidLoad() {
        super.viewDidLoad()
        // Do any additional setup after loading the view, typically from a nib.
        longPressGesture = UILongPressGestureRecognizer(target: self, action: #selector(self.handleLongGesture(gesture:)))
        collectionview.addGestureRecognizer(longPressGesture)

    }


 @objc func handleLongGesture(gesture: UILongPressGestureRecognizer) {
        switch(gesture.state) {

        case .began:
            guard let selectedIndexPath = collectionview.indexPathForItem(at: gesture.location(in: collectionview)) else {
                break
            }
            collectionview.beginInteractiveMovementForItem(at: selectedIndexPath)
        case .changed:
            collectionview.updateInteractiveMovementTargetPosition(gesture.location(in: gesture.view!))
        case .ended:
            collectionview.endInteractiveMovement()
        default:
            collectionview.cancelInteractiveMovement()
        }
    }
