# barcode-detection

<img width="300" alt="スクリーンショット 2022-12-31 9 58 52" src="https://user-images.githubusercontent.com/47273077/210120338-3452eaa2-ad49-444b-8f63-3bbd559eb3b5.gif">

```swift
    private func performBarcodeDetection(completion: @escaping ([VNBarcodeObservation]?) -> Void) {
        
        guard let image = UIImage(named: photos[currentIndex]),
              let orientation = CGImagePropertyOrientation(rawValue: UInt32(image.imageOrientation.rawValue)),
            let cgImage = image.cgImage else {
                return completion(nil)
        }
        
        let request = VNDetectBarcodesRequest { (request, error) in
            
            let observations = request.results as? [VNBarcodeObservation]
            completion(observations)
        }
        
        let handler = VNImageRequestHandler(cgImage: cgImage, orientation: orientation, options: [:])
        
        do {
            try handler.perform([request])
        } catch {
            print(error.localizedDescription)
        }
        
    }

 // ------------------
 
    Button("Classify") {
                
                self.performBarcodeDetection { observations in
                    
                    guard let observations = observations,
                        let observation = observations.first else {
                            return
                    }
                    
                    if let payload = observation.payloadStringValue {
                        self.classification = payload
                    }
                    
                }
                
            }
```
