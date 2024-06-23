# vijay
Capcha Generator
import java.awt.*;
import java.awt.image.BufferedImage;
import java.io.File;
import java.io.IOException;
import java.util.Random;
import javax.imageio.ImageIO;
import javax.swing.*;

public class CaptchaGenerator {

    private static final String CHARACTERS = "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
    private static final int CAPTCHA_LENGTH = 6;
    private static final int WIDTH = 160;
    private static final int HEIGHT = 60;

    public static String generateCaptchaText() {
        StringBuilder captcha = new StringBuilder();
        Random random = new Random();

        for (int i = 0; i < CAPTCHA_LENGTH; i++) {
            int index = random.nextInt(CHARACTERS.length());
            captcha.append(CHARACTERS.charAt(index));
        }

        return captcha.toString();
    }

    public static BufferedImage generateCaptchaImage(String captchaText) {
        BufferedImage image = new BufferedImage(WIDTH, HEIGHT, BufferedImage.TYPE_INT_RGB);
        Graphics2D graphics = image.createGraphics();

        // Background color
        graphics.setColor(Color.WHITE);
        graphics.fillRect(0, 0, WIDTH, HEIGHT);

        // Font settings
        graphics.setFont(new Font("Arial", Font.BOLD, 40));
        graphics.setColor(Color.BLACK);

        // Draw the CAPTCHA text
        graphics.drawString(captchaText, 20, 45);

        // Optional: Add some noise or lines for extra security
        Random random = new Random();
        for (int i = 0; i < 15; i++) {
            int x1 = random.nextInt(WIDTH);
            int y1 = random.nextInt(HEIGHT);
            int x2 = random.nextInt(WIDTH);
            int y2 = random.nextInt(HEIGHT);
            graphics.setColor(new Color(random.nextInt(255), random.nextInt(255), random.nextInt(255)));
            graphics.drawLine(x1, y1, x2, y2);
        }

        graphics.dispose();
        return image;
    }

    public static void main(String[] args) {
        String captchaText = generateCaptchaText();
        System.out.println("Generated CAPTCHA: " + captchaText);

        BufferedImage captchaImage = generateCaptchaImage(captchaText);

        // Save the image to a file
        try {
            ImageIO.write(captchaImage, "png", new File("captcha.png"));
            System.out.println("CAPTCHA image saved as captcha.png");
        } catch (IOException e) {
            e.printStackTrace();
        }

        // Display the image in a simple GUI
        JFrame frame = new JFrame();
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.setSize(WIDTH, HEIGHT);
        JLabel label = new JLabel(new ImageIcon(captchaImage));
        frame.add(label);
        frame.setVisible(true);
    }
}
