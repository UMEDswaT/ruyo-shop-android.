package tj.ruyoshop.ads;

import android.app.Activity;
import android.content.ClipData;
import android.content.ClipboardManager;
import android.content.Context;
import android.graphics.Color;
import android.graphics.Typeface;
import android.os.Bundle;
import android.text.InputType;
import android.view.Gravity;
import android.view.View;
import android.widget.Button;
import android.widget.EditText;
import android.widget.LinearLayout;
import android.widget.ScrollView;
import android.widget.TextView;
import android.widget.Toast;

public class MainActivity extends Activity {
    private final int GOLD = Color.rgb(216, 173, 88);
    private final int INK = Color.rgb(15, 15, 20);
    private EditText name, category, price, oldPrice, color, benefits, notes;
    private LinearLayout results;

    @Override public void onCreate(Bundle state) {
        super.onCreate(state);
        getWindow().setStatusBarColor(Color.rgb(10,10,13));

        ScrollView scroll = new ScrollView(this);
        scroll.setBackgroundColor(Color.rgb(247,244,238));
        LinearLayout root = column(18);
        root.setPadding(dp(18), dp(22), dp(18), dp(36));
        scroll.addView(root);

        TextView brand = text("RUYO SHOP", 28, INK, true);
        brand.setLetterSpacing(.13f);
        root.addView(brand);
        TextView sub = text("Генератори реклама", 16, Color.DKGRAY, false);
        sub.setPadding(0, dp(3), 0, dp(20)); root.addView(sub);

        root.addView(section("МАЪЛУМОТИ МАҲСУЛОТ"));
        name = field(root, "Номи маҳсулот *", "Масалан: сумкаи занона");
        category = field(root, "Категория *", "Сумка, либос, тӯҳфа...");
        price = field(root, "Нархи нав", "350 сомонӣ");
        oldPrice = field(root, "Нархи пешина", "450 сомонӣ");
        color = field(root, "Ранг", "Сиёҳ");
        benefits = field(root, "Бартариҳо", "Васеъ, сабук, босифат");
        notes = field(root, "Эзоҳ", "Услуби premium");

        Button generate = button("СОХТАНИ РЕКЛАМА");
        generate.setOnClickListener(v -> generate());
        LinearLayout.LayoutParams bp = new LinearLayout.LayoutParams(-1, dp(56));
        bp.setMargins(0, dp(12), 0, dp(22)); root.addView(generate, bp);

        results = column(14);
        results.setVisibility(View.GONE);
        root.addView(results);

        TextView foot = text("RUYO SHOP • ВЕРСИЯИ 1.0", 12, Color.GRAY, true);
        foot.setGravity(Gravity.CENTER); foot.setPadding(0, dp(28), 0, 0);
        root.addView(foot);
        setContentView(scroll);
    }

    private void generate() {
        String n = val(name), c = val(category);
        if (n.isEmpty() || c.isEmpty()) {
            Toast.makeText(this, "Ном ва категорияро пур кунед", Toast.LENGTH_SHORT).show(); return;
        }
        String p = val(price), op = val(oldPrice), col = val(color), ben = val(benefits), note = val(notes);
        String facts = "Product: " + n + ". Category: " + c +
                (col.isEmpty()?"":". Color: "+col) + (ben.isEmpty()?"":". Features: "+ben) +
                (note.isEmpty()?"":". Notes: "+note) + ". ";
        String preserve = "Preserve the exact original product, packaging, logo, label, colors, proportions and readable text. No redesign, no distortion, no extra objects.";
        String image = facts + "Premium luxury e-commerce product photography for RUYO SHOP, square 1:1, hero product centered, black and warm gold accents, clean elegant background, realistic materials, soft studio lighting, refined shadows, high contrast, crisp commercial detail, 8K. " + preserve;
        String image45 = facts + "Premium Instagram product advertisement, portrait 4:5, product occupying 60% of frame, sophisticated black, cream and gold palette, editorial composition, generous negative space, realistic studio reflections, soft cinematic light, conversion-focused, 8K. " + preserve;
        String video = facts + "Animate the supplied product photo into a premium 9:16 Instagram Reel, 7 seconds. Slow camera push-in with subtle parallax, elegant light sweep and realistic shadow movement, clean softly blurred luxury background. Keep product completely stable and recognizable. No morphing, no added text, no logo changes, no distortion.";
        String discount = (!p.isEmpty() && !op.isEmpty()) ? " Нархи пешина: " + op + ". Ҳоло: " + p + "." : (!p.isEmpty()?" Нарх: "+p+".":"");
        String discountRu = (!p.isEmpty() && !op.isEmpty()) ? " Старая цена: " + op + ". Сейчас: " + p + "." : (!p.isEmpty()?" Цена: "+p+".":"");
        String capTj = "Зебоӣ ва сифат дар ҳар ҷузъ ✨ " + n + (ben.isEmpty()?".":" — "+ben+".") + discount + " Барои фармоиш ба Direct нависед.";
        String capRu = "Красота и качество в каждой детали ✨ " + n + (ben.isEmpty()?".":" — "+ben+".") + discountRu + " Для заказа напишите в Direct.";

        results.removeAllViews();
        results.addView(section("НАТИҶАИ ТАЙЁР"));
        card("Промти акс 1:1", image);
        card("Промти акс 4:5", image45);
        card("Промти видео 9:16", video);
        card("Caption — тоҷикӣ", capTj);
        card("Caption — русский", capRu);
        card("CTA", "Барои фармоиш ба Direct нависед • Для заказа напишите в Direct");
        card("Hashtag", "#RUYOSHOP #Dushanbe #Тоҷикистон #ЖенскийСтиль #ОнлайнМагазин #Подарки");
        results.setVisibility(View.VISIBLE);
        results.requestFocus();
    }

    private void card(String title, String value) {
        LinearLayout box = column(10); box.setPadding(dp(16),dp(15),dp(16),dp(15));
        box.setBackgroundColor(Color.WHITE); box.setElevation(dp(2));
        box.addView(text(title, 14, GOLD, true));
        TextView body = text(value, 15, INK, false); body.setTextIsSelectable(true); box.addView(body);
        Button copy = button("НУСХАБАРДОРӢ"); copy.setTextSize(12);
        copy.setOnClickListener(v -> copy(value));
        LinearLayout.LayoutParams cp = new LinearLayout.LayoutParams(-1, dp(42)); cp.setMargins(0,dp(8),0,0);
        box.addView(copy, cp);
        results.addView(box, new LinearLayout.LayoutParams(-1,-2));
    }

    private void copy(String value) {
        ClipboardManager cm=(ClipboardManager)getSystemService(Context.CLIPBOARD_SERVICE);
        cm.setPrimaryClip(ClipData.newPlainText("RUYO SHOP", value));
        Toast.makeText(this,"Нусхабардорӣ шуд",Toast.LENGTH_SHORT).show();
    }
    private EditText field(LinearLayout parent, String label, String hint) {
        TextView l=text(label,14,INK,true); l.setPadding(0,dp(11),0,dp(5)); parent.addView(l);
        EditText e=new EditText(this); e.setHint(hint); e.setTextSize(16); e.setTextColor(INK);
        e.setHintTextColor(Color.rgb(145,145,150)); e.setSingleLine(false); e.setMinHeight(dp(52));
        e.setPadding(dp(14),0,dp(14),0); e.setBackgroundColor(Color.WHITE);
        if(label.contains("Нарх")) e.setInputType(InputType.TYPE_CLASS_TEXT);
        parent.addView(e,new LinearLayout.LayoutParams(-1,-2)); return e;
    }
    private Button button(String label) {
        Button b=new Button(this); b.setText(label); b.setTextColor(Color.BLACK); b.setTextSize(14);
        b.setTypeface(Typeface.DEFAULT,Typeface.BOLD); b.setBackgroundColor(GOLD); b.setAllCaps(false); return b;
    }
    private TextView section(String s) { TextView t=text(s,13,GOLD,true); t.setLetterSpacing(.08f); return t; }
    private TextView text(String s,int size,int color,boolean bold) {
        TextView t=new TextView(this); t.setText(s); t.setTextSize(size); t.setTextColor(color);
        t.setLineSpacing(0,1.18f); if(bold)t.setTypeface(Typeface.DEFAULT,Typeface.BOLD); return t;
    }
    private LinearLayout column(int gap) { LinearLayout l=new LinearLayout(this); l.setOrientation(LinearLayout.VERTICAL); l.setShowDividers(LinearLayout.SHOW_DIVIDER_MIDDLE); l.setDividerPadding(dp(gap)); return l; }
    private String val(EditText e){return e.getText().toString().trim();}
    private int dp(int n){return Math.round(n*getResources().getDisplayMetrics().density);}
}
