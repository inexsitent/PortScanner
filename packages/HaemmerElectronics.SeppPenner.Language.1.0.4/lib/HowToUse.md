## Basic usage:
If you just have one language loaded, you don't need to set the current language as
the language manager uses the only language as default.
```csharp
public void Test()
{
    ILanguageManager _lm = new LanguageManager();
    var test = _lm.GetCurrentLanguage().GetWord("YourKey");
}
```
If you have more than one language loaded, you need to set the current (wanted) language.
The language identifiers are taken from https://msdn.microsoft.com/de-de/library/ee825488(v=cs.20).aspx.
```csharp
public void Test()
{
    ILanguageManager _lm = new LanguageManager();
    _lm.SetCurrentLanguage("de-DE"); //Version 1.0.0.0 to Version 1.0.0.3
    _lm.SetCurrentLanguageFromName("German"); //Version 1.0.0.4 and above
    var test = _lm.GetCurrentLanguage().GetWord("YourKey");
}
```

## Subscription to OnLanguageChanged Version 1.0.0.4 and above:
You can "subscribe" to the EventHandler OnLanguageChanged to update your GUI whenever the language is changed.
This can be done like the following: (For more information see [the example project](https://github.com/SeppPenner/CSharpLanguageManager/blob/master/Language/TestLanguage/Main.cs))
```csharp
namespace TestLanguage
{
    public partial class Main : Form
    {
        private readonly ILanguageManager _lm = new LanguageManager();
        private ILanguage _lang;

        public Main()
        {
            InitializeComponent();
            InitializeLanguageManager();
            LoadLanguagesToCombo();
        }

        private void InitializeLanguageManager()
        {
            _lm.SetCurrentLanguage("de-DE");
            _lm.OnLanguageChanged += OnLanguageChanged;
        }

        private void LoadLanguagesToCombo()
        {
            foreach (var lang in _lm.GetLanguages())
                comboBoxLanguage.Items.Add(lang.Name);
            comboBoxLanguage.SelectedIndex = 0;
        }

        private void comboBoxLanguage_SelectedIndexChanged(object sender, EventArgs e)
        {
            _lm.SetCurrentLanguageFromName(comboBoxLanguage.SelectedItem.ToString());
        }

        private void OnLanguageChanged(object sender, EventArgs eventArgs)
        {
            _lang = _lm.GetCurrentLanguage();
            labelSelectLanguage.Text = _lang.GetWord("SelectLanguage");
        }
    }
}
```
where the basic change is that the _lm.SetCurrentLanguage() handling was simplified by setting it via the language name, too:
```csharp
_lm.SetCurrentLanguageFromName(comboBoxLanguage.SelectedItem.ToString());
```

## Subscription to OnLanguageChanged Version 1.0.0.0 to 1.0.0.3:
You can "subscribe" to the EventHandler OnLanguageChanged to update your GUI whenever the language is changed.
This can be done like the following: (For more information see [the example project](https://github.com/SeppPenner/CSharpLanguageManager/blob/master/Language/TestLanguage/Main.cs))

```csharp
using System;
using System.Windows.Forms;
using Languages.Implementation;
using Languages.Interfaces;

namespace TestLanguage
{
    public partial class Main : Form
    {
        private readonly ILanguageManager _lm = new LanguageManager();
        private ILanguage _lang;

        public Main()
        {
            InitializeComponent();
            InitializeLanguageManager();
            LoadLanguagesToCombo();
        }

        private void InitializeLanguageManager()
        {
            _lm.SetCurrentLanguage("de-DE");
            _lm.OnLanguageChanged += OnLanguageChanged;
        }

        private void LoadLanguagesToCombo()
        {
            foreach (var lang in _lm.GetLanguages())
                comboBoxLanguage.Items.Add(lang.Name);
            comboBoxLanguage.SelectedIndex = 0;
        }

        private void comboBoxLanguage_SelectedIndexChanged(object sender, EventArgs e)
        {
            switch (comboBoxLanguage.SelectedItem.ToString())
            {
                case "German":
                    _lm.SetCurrentLanguage("de-DE");
                    break;
                case "English (US)":
                    _lm.SetCurrentLanguage("en-US");
                    break;
            }
        }

        private void OnLanguageChanged(object sender, EventArgs eventArgs)
        {
            _lang = _lm.GetCurrentLanguage();
            labelSelectLanguage.Text = _lang.GetWord("SelectLanguage");
        }
    }
}
```

## How do the language files need to look like:
The file's naming is not important but should be something like "de-DE.xml".
All language files need to be included into the "languages" folder in your application folder.
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<Language xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:xsd="http://www.w3.org/2001/XMLSchema">
    <!--According to https://msdn.microsoft.com/de-de/library/ee825488(v=cs.20).aspx-->
    <Identifier>de-DE</Identifier> 
    <Name>German<Name>
    <Words>
        <Word>
            <Key>YourKey</Key>
            <Value>TestString</Value>
        </Word>
    </Words>
</Language>
```

An example project can be found [here](https://github.com/SeppPenner/CSharpLanguageManager/tree/master/Language/TestLanguage).
The project can be found on [nuget](https://www.nuget.org/packages/HaemmerElectronics.SeppPenner.Language).