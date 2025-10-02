import React, { useState } from 'react';
import { Card, CardContent, CardHeader, CardTitle } from '@/components/ui/card';
import { Button } from '@/components/ui/button';
import { Input } from '@/components/ui/input';
import { Label } from '@/components/ui/label';
import { ArrowLeft, Mail, CheckCircle } from 'lucide-react';
import { toast } from 'sonner';
import { Link } from 'react-router-dom';

interface User {
  email: string;
  password: string;
  name?: string;
  [key: string]: unknown;
}

export default function ForgotPassword() {
  const [email, setEmail] = useState('');
  const [isLoading, setIsLoading] = useState(false);
  const [resetCode, setResetCode] = useState('');
  const [newPassword, setNewPassword] = useState('');
  const [confirmPassword, setConfirmPassword] = useState('');
  const [step, setStep] = useState(1); // 1: Email, 2: Code, 3: New Password

  const handleSendResetEmail = async (e: React.FormEvent) => {
    e.preventDefault();
    
    if (!email) {
      toast.error('Please enter your email address');
      return;
    }

    // Email validation
    const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
    if (!emailRegex.test(email)) {
      toast.error('Please enter a valid email address');
      return;
    }

    setIsLoading(true);

    // Simulate API call
    setTimeout(() => {
      // Generate a mock reset code
      const mockResetCode = Math.floor(100000 + Math.random() * 900000).toString();
      localStorage.setItem('resetCode', mockResetCode);
      localStorage.setItem('resetEmail', email);
      
      setStep(2);
      setIsLoading(false);
      toast.success('Reset code sent to your email!');
      
      // For demo purposes, show the code in console
      console.log('Reset Code (for demo):', mockResetCode);
      toast.info(`Demo Reset Code: ${mockResetCode}`, { duration: 10000 });
    }, 2000);
  };

  const handleVerifyCode = (e: React.FormEvent) => {
    e.preventDefault();
    
    const storedCode = localStorage.getItem('resetCode');
    const storedEmail = localStorage.getItem('resetEmail');
    
    if (!resetCode) {
      toast.error('Please enter the reset code');
      return;
    }
    
    if (resetCode !== storedCode || email !== storedEmail) {
      toast.error('Invalid reset code. Please try again.');
      return;
    }
    
    setStep(3);
    toast.success('Code verified! Please set your new password.');
  };

  const handleResetPassword = (e: React.FormEvent) => {
    e.preventDefault();
    
    if (!newPassword || !confirmPassword) {
      toast.error('Please fill in all password fields');
      return;
    }
    
    if (newPassword.length < 6) {
      toast.error('Password must be at least 6 characters long');
      return;
    }
    
    if (newPassword !== confirmPassword) {
      toast.error('Passwords do not match');
      return;
    }
    
    setIsLoading(true);
    
    // Simulate password reset
    setTimeout(() => {
      // Update user password in localStorage (in real app, this would be API call)
      const users: User[] = JSON.parse(localStorage.getItem('registeredUsers') || '[]');
      const userIndex = users.findIndex((user: User) => user.email === email);
      
      if (userIndex !== -1) {
        users[userIndex].password = newPassword;
        localStorage.setItem('registeredUsers', JSON.stringify(users));
      }
      
      // Clean up reset data
      localStorage.removeItem('resetCode');
      localStorage.removeItem('resetEmail');
      
      setIsLoading(false);
      toast.success('Password reset successfully! You can now sign in with your new password.');
      
      // Redirect to sign in after 2 seconds
      setTimeout(() => {
        window.location.href = '/signin';
      }, 2000);
    }, 1500);
  };

  const resendCode = () => {
    handleSendResetEmail({ preventDefault: () => {} } as React.FormEvent);
  };

  return (
    <div className="min-h-screen bg-gradient-to-br from-gray-900 via-blue-900 to-gray-900 flex items-center justify-center p-4">
      <Card className="w-full max-w-md bg-gray-800 border-gray-700">
        <CardHeader className="text-center">
          <div className="flex items-center justify-center mb-4">
            <div className="w-12 h-12 bg-gradient-to-r from-blue-500 to-purple-600 rounded-lg flex items-center justify-center">
              <Mail className="w-6 h-6 text-white" />
            </div>
          </div>
          <CardTitle className="text-2xl font-bold text-white">
            {step === 1 && 'Reset Password'}
            {step === 2 && 'Verify Code'}
            {step === 3 && 'New Password'}
          </CardTitle>
          <p className="text-gray-400 mt-2">
            {step === 1 && 'Enter your email to receive a reset code'}
            {step === 2 && 'Enter the 6-digit code sent to your email'}
            {step === 3 && 'Create a new password for your account'}
          </p>
        </CardHeader>

        <CardContent className="space-y-4">
          {step === 1 && (
            <form onSubmit={handleSendResetEmail} className="space-y-4">
              <div className="space-y-2">
                <Label htmlFor="email" className="text-white">Email Address</Label>
                <Input
                  id="email"
                  type="email"
                  value={email}
                  onChange={(e) => setEmail(e.target.value)}
                  placeholder="Enter your email address"
                  className="bg-gray-700 border-gray-600 text-white placeholder-gray-400"
                  required
                />
              </div>

              <Button 
                type="submit" 
                className="w-full bg-blue-600 hover:bg-blue-700"
                disabled={isLoading}
              >
                {isLoading ? 'Sending...' : 'Send Reset Code'}
              </Button>
            </form>
          )}

          {step === 2 && (
            <form onSubmit={handleVerifyCode} className="space-y-4">
              <div className="text-center mb-4">
                <CheckCircle className="w-12 h-12 text-green-500 mx-auto mb-2" />
                <p className="text-green-400 text-sm">Reset code sent to {email}</p>
              </div>

              <div className="space-y-2">
                <Label htmlFor="resetCode" className="text-white">Reset Code</Label>
                <Input
                  id="resetCode"
                  type="text"
                  value={resetCode}
                  onChange={(e) => setResetCode(e.target.value)}
                  placeholder="Enter 6-digit code"
                  className="bg-gray-700 border-gray-600 text-white placeholder-gray-400 text-center text-lg tracking-widest"
                  maxLength={6}
                  required
                />
              </div>

              <Button 
                type="submit" 
                className="w-full bg-blue-600 hover:bg-blue-700"
              >
                Verify Code
              </Button>

              <Button 
                type="button" 
                variant="ghost"
                className="w-full text-gray-400 hover:text-white"
                onClick={resendCode}
              >
                Resend Code
              </Button>
            </form>
          )}

          {step === 3 && (
            <form onSubmit={handleResetPassword} className="space-y-4">
              <div className="space-y-2">
                <Label htmlFor="newPassword" className="text-white">New Password</Label>
                <Input
                  id="newPassword"
                  type="password"
                  value={newPassword}
                  onChange={(e) => setNewPassword(e.target.value)}
                  placeholder="Enter new password"
                  className="bg-gray-700 border-gray-600 text-white placeholder-gray-400"
                  required
                />
              </div>

              <div className="space-y-2">
                <Label htmlFor="confirmPassword" className="text-white">Confirm Password</Label>
                <Input
                  id="confirmPassword"
                  type="password"
                  value={confirmPassword}
                  onChange={(e) => setConfirmPassword(e.target.value)}
                  placeholder="Confirm new password"
                  className="bg-gray-700 border-gray-600 text-white placeholder-gray-400"
                  required
                />
              </div>

              <div className="bg-gray-700 p-3 rounded-lg">
                <p className="text-gray-300 text-sm">Password requirements:</p>
                <ul className="text-gray-400 text-xs mt-1 space-y-1">
                  <li>• At least 6 characters long</li>
                  <li>• Must match confirmation</li>
                </ul>
              </div>

              <Button 
                type="submit" 
                className="w-full bg-green-600 hover:bg-green-700"
                disabled={isLoading}
              >
                {isLoading ? 'Resetting...' : 'Reset Password'}
              </Button>
            </form>
          )}

          <div className="text-center pt-4 border-t border-gray-700">
            <Link 
              to="/signin" 
              className="inline-flex items-center text-blue-400 hover:text-blue-300 text-sm"
            >
              <ArrowLeft className="w-4 h-4 mr-1" />
              Back to Sign In
            </Link>
          </div>
        </CardContent>
      </Card>
    </div>
  );
}
