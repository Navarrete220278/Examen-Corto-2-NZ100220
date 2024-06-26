# Examen-Corto-2-NZ100220
CRIPTOGRAFIA

import java.math.BigInteger;
import java.util.Random;

public class RSA {

    private BigInteger p;
    private BigInteger q;
    private BigInteger n;
    private BigInteger phi;
    private BigInteger e;
    private BigInteger d;
    private int bitlength = 1024;
    private Random r;

    public RSA() {
        r = new Random();
        p = BigInteger.probablePrime(bitlength, r);
        q = BigInteger.probablePrime(bitlength, r);
        n = p.multiply(q);
        phi = p.subtract(BigInteger.ONE).multiply(q.subtract(BigInteger.ONE));
        e = BigInteger.probablePrime(bitlength / 2, r);
        while (phi.gcd(e).compareTo(BigInteger.ONE) > 0 && e.compareTo(phi) < 0) {
            e.add(BigInteger.ONE);
        }
        d = e.modInverse(phi);
    }

    public RSA(BigInteger e, BigInteger d, BigInteger n) {
        this.e = e;
        this.d = d;
        this.n = n;
    }

    public static void main(String[] args) {
        RSA rsa = new RSA();
        String plaintext = "Mauricio Navarrete, NZ100220";
        System.out.println("Plaintext: " + plaintext);

        // Cifrar
        BigInteger plaintextBigInt = new BigInteger(plaintext.getBytes());
        BigInteger ciphertext = rsa.encrypt(plaintextBigInt);
        System.out.println("Ciphertext: " + ciphertext);

        // Descifrar
        BigInteger decryptedPlaintext = rsa.decrypt(ciphertext);
        System.out.println("Decrypted plaintext: " + new String(decryptedPlaintext.toByteArray()));
    }

    public BigInteger encrypt(BigInteger message) {
        return message.modPow(e, n);
    }

    public BigInteger decrypt(BigInteger encryptedMessage) {
        return encryptedMessage.modPow(d, n);
    }
}
