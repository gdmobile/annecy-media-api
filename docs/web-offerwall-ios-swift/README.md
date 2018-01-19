# Annecy Web Offerwall (iOS Swift)

Use our [API docs](https://admin.annecy.media/docs) for an awesome integration experience!

## Example

You can get your custom web offerwall [here](https://admin.annecy.media/offerwall). Create a WebView and make sure that clicked offers will open in Safari!

``` swift
import UIKit
import AdSupport

class ViewController: UIViewController, UIWebViewDelegate {
    override func viewDidLoad() {
        super.viewDidLoad()

        // Set your publisher token and user ID
        let TOKEN = "6ce0bbf0-2dc8-4d7c-a497-e93105188ba1"
        let USER_ID = "foo"

        // Get user settings
        let country = Locale.current.regionCode ?? ""
        let language = Locale.preferredLanguages[0]
        let idfa = ASIdentifierManager.shared().isAdvertisingTrackingEnabled
            ? ASIdentifierManager.shared().advertisingIdentifier.uuidString
            : ""

        // Create an Annecy WebWiew
        let annecyWebVew:UIWebView = UIWebView(frame: CGRect(x:0, y:0, width: UIScreen.main.bounds.width, height:UIScreen.main.bounds.height))

        self.view.addSubview(annecyWebView)
        annecyWebView.delegate = self

        let annecyURL = URL(string: "https://offerwall.annecy.media?country=\(country)&idfa_gaid=\(idfa)&language=\(language)&token=\(TOKEN)&user_id=\(USER_ID)&platform=ios")
        let annecyURLRequest:URLRequest = URLRequest(url: annecyURL!)
        annecyWebView.loadRequest(annecyURLRequest)
    }

    public func webView(_ webView: UIWebView, shouldStartLoadWith request: URLRequest, navigationType: UIWebViewNavigationType) -> Bool {

        // Open offers in Safari
        if navigationType == .linkClicked {
            if #available(iOS 10.0, *) {
                UIApplication.shared.open(request.url!, options: [:], completionHandler: nil)
            } else {
                UIApplication.shared.openURL(request.url!)
            }

            return false
        }

        return true
    }

    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
    }
}
```