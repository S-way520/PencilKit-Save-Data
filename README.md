# PencilKit-Save-Data
PencilKit and SwiftUI Save Data
# The first part
![IMG_0468](https://github.com/S-way520/PencilKit-Save-Data/assets/95877651/b739e147-5a15-450c-9c56-09848068f236)
# The second part
Code file 1
```swift
import SwiftUI
@main
struct MyApp: App {
    var body: some Scene {
        WindowGroup {
            ContentView()
        }
    }
}
```
Code file 2
```swift
import SwiftUI
import PencilKit
struct ContentView: View {
    @AppStorage("SavedData") var savedDrawingData: Data?
    @State private var canvasView = PKCanvasView()
    @State private var drawing = PKDrawing()
    var body: some View {
        NavigationView {
            VStack {
                PKCanvasViewWrapper(canvasView: $canvasView, drawing: $drawing)
                Button(action: { saveDrawing() }) {
                    Text("保存")
                }
            }
            .onAppear(perform: { loadDrawing() })
            .navigationBarTitle("绘图")
        }
    }
    func saveDrawing() {
        savedDrawingData = canvasView.drawing.dataRepresentation()
    }
    func loadDrawing() {
        if let savedData = savedDrawingData {
            if let loadedDrawing = try? PKDrawing(data: savedData) {
                drawing = loadedDrawing
            }
        }
    }
}
struct PKCanvasViewWrapper: UIViewRepresentable {
    @Binding var canvasView: PKCanvasView
    @Binding var drawing: PKDrawing
    func makeUIView(context: Context) -> PKCanvasView {
        canvasView.drawing = drawing
        canvasView.drawingPolicy = .anyInput
        return canvasView
    }
    func updateUIView(_ uiView: PKCanvasView, context: Context) {
        uiView.drawing = drawing
    }
}
```
